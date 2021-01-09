# How Kafka consumer manages partition offsets?

## Question

Kafka Consumer가 offset commit을 실패하면 다음 poll할 때 실패하기 전의 offset을 기준으로 레코드를 다시 가져올까? 즉 같은 레코드 목록을 다시 가져올까? 아니면 commit 실패와 상관 없이 다음 레코드 목록을 가져올까? 

이를 알아보기 위해 Kafka 공식 라이브러리의 `KafkaConsumer.poll()` 동작 방식을 알아보자. 라이브러리는 2.7.x 버전을 기준으로 했다.

## Answer

먼저 `KafkaConsumer.poll()`가 호출하는 `ConsumerRecords<K, V> poll(final Timer timer, final boolean includeMetadataInTimeout)`로 가보자. 코드 예제는 가독성을 위해 중요하지 않은 코드는 전부 생략한다.

```java
    private ConsumerRecords<K, V> poll(final Timer timer, final boolean includeMetadataInTimeout) {
        try {
            do {
                // try to update assignment metadata BUT do not need to block on the timer for join group
                updateAssignmentMetadataIfNeeded(timer, false);

                final Map<TopicPartition, List<ConsumerRecord<K, V>>> records = pollForFetches(timer);
            } while (timer.notExpired());

            return ConsumerRecords.empty();
        } finally {
            release();
        }
    }
```

우선 `updateAssignmentMetadataIfNeeded()`를 먼저 보자. 해당 메소드의 핵심은 `updateFetchPositions()` 호출이다. 

```java
    private boolean updateFetchPositions(final Timer timer) {
        // If any partitions have been truncated due to a leader change, we need to validate the offsets
        fetcher.validateOffsetsIfNeeded();

        cachedSubscriptionHashAllFetchPositions = subscriptions.hasAllFetchPositions();
        if (cachedSubscriptionHashAllFetchPositions) return true;

        // If there are any partitions which do not have a valid position and are not
        // awaiting reset, then we need to fetch committed offsets. We will only do a
        // coordinator lookup if there are partitions which have missing positions, so
        // a consumer with manually assigned partitions can avoid a coordinator dependence
        // by always ensuring that assigned partitions have an initial position.
        if (coordinator != null && !coordinator.refreshCommittedOffsetsIfNeeded(timer)) return false;

        // If there are partitions still needing a position and a reset policy is defined,
        // request reset using the default policy. If no reset strategy is defined and there
        // are partitions with a missing position, then we will raise an exception.
        subscriptions.resetInitializingPositions();

        // Finally send an asynchronous request to lookup and update the positions of any
        // partitions which are awaiting reset.
        fetcher.resetOffsetsIfNeeded();

        return true;
    }
```

해당 메소드는`coordinator.refreshCommittedOffsetsIfNeeded()`을 호출해서 Coordinator(= Broker)로부터 Consumer가 가져오고 싶은 Topic Partition에 대한 해당 consumer group의 committed offset 목록을 받아온다.

(참고: For each consumer group, one of the brokers is selected as the group coordinator)

주석을 보면 알겠지만 이 정보는 캐싱해놓는다. 다시 말해, 매번 `poll()`할 때마다 새로 가져오지 않는다.

그 다음 `subscriptions.resetInitializingPositions()`와 `fetcher.resetOffsetsIfNeeded()`를 호출해서 offset position을 업데이트한다. 여기에서 `Fetcher`는 브로커와의 통신(fetch)을 담당하는 클래스이다.

그런데 뭘 어디에 업데이트한다는 것인가? 먼저 `subscriptions` 멤버 변수가 뭔지 보자.

```java
private final SubscriptionState subscriptions;
```

그렇다면 `SubscriptionState`는 뭘까? 해당 클래스의 주석을 보면 `A class for tracking the topics, partitions, and offsets for the consumer.`라고 나와있다. 즉, `KafkaConsumer`를 위해 토픽, 파티션, offset 정보를 트래킹하는 클래스이다. 이 클래스에서 중요한 멤버 변수는 assignment이다.

```java
/* the partitions that are currently assigned, note that the order of partition matters (see FetchBuilder for more details) */
private final PartitionStates<TopicPartitionState> assignment;
```

주석을 보면 알 수 있듯이 현재 할당된 파티션 정보를 담는 변수이다. `PartitionStates`은 타입 파라미터로 생성된 인스턴스를 Map과 Set으로 관리한다. 여기서 핵심은 `TopicPartitionState`이다.

```java
    private static class TopicPartitionState {

        private FetchState fetchState;
        private FetchPosition position; // last consumed position

        private Long highWatermark; // the high watermark from last fetch
        private Long logStartOffset; // the log start offset
        private Long lastStableOffset;
        private boolean paused;  // whether this partition has been paused by the user
        private OffsetResetStrategy resetStrategy;  // the strategy to use if the offset needs resetting
        private Long nextRetryTimeMs;
        private Integer preferredReadReplica;
        private Long preferredReadReplicaExpireTimeMs;
        
        private void position(FetchPosition position) {
            if (!hasValidPosition())
                throw new IllegalStateException("Cannot set a new position without a valid current position");
            this.position = position;
        }
    }
```

`FetchPosition position`가 마지막으로 consume한 position을 담고 있는 멤버 변수이다. 여태까지의 여정을 정리하면 `KafkaConsumer`가 `SubscriptionState` 멤버 변수를 가지고 있고, `SubscriptionState`가 `FetchPosition` 멤버 변수를 가지고 있다. 이를 통해 `KafkaConsumer`는 초기 `poll()` 호출 이후에는 `TopicPartition`별 position을 로컬 메모리에서 관리하고 있음을 유추할 수 있다.

여담으로, consumer group 기준으로 브로커에 특정 `TopicPartition`에 대한 offset 정보가 없다면 `auto.offset.reset` 설정에 따라 상태를 저장한다. `latest` 혹은 `earliest` 옵션이 바로 이것이다.

`KafkaConsumer.updateAssignmentMetadataIfNeeded()` 분석은 이쯤에서 마무리하고, 이제 브로커로부터 레코드를 가져오는 `KafkaConsumer. pollForFetches()`을 분석해보자.

```java
    private Map<TopicPartition, List<ConsumerRecord<K, V>>> pollForFetches(Timer timer) {
        long pollTimeout = coordinator == null ? timer.remainingMs() :
                Math.min(coordinator.timeToNextPoll(timer.currentTimeMs()), timer.remainingMs());

        // if data is available already, return it immediately
        final Map<TopicPartition, List<ConsumerRecord<K, V>>> records = fetcher.fetchedRecords();
        if (!records.isEmpty()) {
            return records;
        }

        // send any new fetches (won't resend pending fetches)
        fetcher.sendFetches();

        ....
        
        return fetcher.fetchedRecords();
    }
```

여기에서 fetch된 레코드 목록을 리턴하는 `Fetcher.fetchedRecords()`를 보자. 참고로, `Fetcher` 클래스는 `SubscriptionState` 클래스와 마찬가지로 `KafkaConsumer` 인스턴스가 생성될 때 같이 생성되는 클래스이다. 즉, `KafkaConsumer` 하나 당 `Fetcher` 하나가 할당된다.

```java
    public Map<TopicPartition, List<ConsumerRecord<K, V>>> fetchedRecords() {
        Map<TopicPartition, List<ConsumerRecord<K, V>>> fetched = new HashMap<>();
        Queue<CompletedFetch> pausedCompletedFetches = new ArrayDeque<>();
        int recordsRemaining = maxPollRecords;

        try {
            while (recordsRemaining > 0) {
                if (nextInLineFetch == null || nextInLineFetch.isConsumed) {
                    ....
                } else if (subscriptions.isPaused(nextInLineFetch.partition)) {
                    ....
                } else {
                    List<ConsumerRecord<K, V>> records = fetchRecords(nextInLineFetch, recordsRemaining);

                    ....
                }
            }
        } catch (KafkaException e) {
            if (fetched.isEmpty())
                throw e;
        } finally {
            completedFetches.addAll(pausedCompletedFetches);
        }

        return fetched;
    }
```

레코드를 fetch하는 코드로 보이는데 관심사는 아니다. 레코드를 가져오는 메소드로 추정되는 `Fetcher.fetchRecords()`로 가자. 

```java
    private List<ConsumerRecord<K, V>> fetchRecords(CompletedFetch completedFetch, int maxRecords) {
        if (!subscriptions.isAssigned(completedFetch.partition)) {
            ....
        } else if (!subscriptions.isFetchable(completedFetch.partition)) {
            ....
        } else {
            FetchPosition position = subscriptions.position(completedFetch.partition); // #1
            if (position == null) {
                throw new IllegalStateException("Missing position for fetchable partition " + completedFetch.partition);
            }

            if (completedFetch.nextFetchOffset == position.offset) {
                List<ConsumerRecord<K, V>> partRecords = completedFetch.fetchRecords(maxRecords); // #2

                if (completedFetch.nextFetchOffset > position.offset) {
                    FetchPosition nextPosition = new FetchPosition(
                            completedFetch.nextFetchOffset,
                            completedFetch.lastEpoch,
                            position.currentLeader);
                    subscriptions.position(completedFetch.partition, nextPosition); // #3
                }

                ....

                return partRecords;
            } else {
                ....
            }
        }

        ....

        return emptyList();
    }
```

코드를 순서대로 따라가보자.

우선 `#1`에서 `subscriptions`에서 특정 파티션에 대한 offset position을 받는다. `subscriptions`는 무엇인가? 이는 인스턴스가 생성될 때 `KafkaConsumer`에서 넘겨준 값이다. 즉, `KafkaConsumer`와 `Fetcher`는 `SubscriptionState` 인스턴스를 공유한다. Position은 `KafkaConsumer.updateAssignmentMetadataIfNeeded()`에서 설정된 값일 것이다.

```java
    public KafkaConsumer(Map<String, Object> configs,
                         Deserializer<K> keyDeserializer,
                         Deserializer<V> valueDeserializer) {
            this.fetcher = new Fetcher<>(
                    ....,
                    this.subscriptions,
                    metrics,
                    metricsRegistry,
                    this.time,
                    this.retryBackoffMs,
                    this.requestTimeoutMs,
                    isolationLevel,
                    apiVersions);
    }
```

`#2`에서는 레코드를 fetch한다. 관심사가 아니므로 세부 사항은 넘어간다.

`#3`은 위 파티션에 position을 다음 offset으로 업데이트하는 메소드이다. 현재 offset + 다음에 fetch할 record 갯수가 `nextPosition`임을 유추해볼 수 있다.

그런데 position은 `TopicPartitionState`에서 관리한다고 하지 않았나? 왜 `SubscriptionState.position()` 메소드가 호출되나? `SubscriptionState` 클래스로 가보자.

```java
    private TopicPartitionState assignedState(TopicPartition tp) {
        TopicPartitionState state = this.assignment.stateValue(tp);
        if (state == null)
            throw new IllegalStateException("No current assignment for partition " + tp);
        return state;
    }
    
    public synchronized void position(TopicPartition tp, FetchPosition position) {
        assignedState(tp).position(position);
    }
    
    public synchronized FetchPosition position(TopicPartition tp) {
        return assignedState(tp).position;
    }
```

`Subscription` 클래스는 단순히 `assignment`의 상태를 대신 조회하거나 갱신함을 알 수 있다.

더 상세하게 분석해볼 수 있겠지만 이쯤 되면 **Kafka Consumer가 offset commit을 실패하면 다음 poll할 때 실패하기 전의 offset을 기준으로 레코드를 다시 가져올까?**에 대한 답을 할 수 있다.

- `KafkaConsumer`는 자체적으로 offset 정보를 가지고 있지 않을 때 `Coordinator`에 조회하여 값을 내부에 저장하고 쓴다. 여기서 중요한 포인트는 값을 `poll()`할 때마다 갱신하지 않는다는 점이다.
- `KafkaConsumer`는 `SubscriptionState`를 이용해 `TopicPartition` 단위로 현재의 offset position을 관리한다.
- `Fetcher`는 레코드들을 브로커로부터 가져올 때 `SubscriptionState`의 offset을 이용하고, 가져오고 나면 다음 offset으로 position을 업데이트한다.
- **즉, offset commit 실패는 레코드를 가져오는 `poll()` 동작에 아무런 영향을 주지 않는다.** 브로커에 commit된 offset은 Consumer 자신을 위한 것이 아니라 본인이 죽고 난 후 작업을 이어 받을 다음 Consumer을 위한 것이다.

## Thoughts

[카프카 핵심 가이드] 90페이지를 보면 "재시도 없이 오프셋 커밋이 실패해도 큰 문제가 되지 않는다. 그것이 일시적이라면 그 다음의 커밋에서 성공할 것이기 때문이다"라는 문장이 있다. 왜 문제가 되지 않는지 궁금해서 찾아보기 시작했는데 웹상에는 상세하게 설명해주는 내용이 없어서 분석해보게 되었다.

분석하면서 느낀 점은 카프카가 내부 동작이 간단하지 않은 플랫폼임에도 불구하고 공식 라이브러리 코드가 굉장히 간결하고 읽기 쉽게 작성되었다는 것이다. 라인마다 주석도 상세히 달려있어서 기본 지식만 있다면 깊게 들어가지 않아도 동작 방식을 이해할 수 있었다.

## References

- <https://chrzaszcz.dev/2019/06/16/kafka-consumer-poll/>