# Transaction isolation level

*이 글의 일부는 학습을 위해 김영한님의 '자바 ORM 표준 JPA 프로그래밍' 695-696페이지를 그대로 적었습니다. 문제가 되면 즉시 삭제하겠습니다.*

## ACID

트랜잭션은 ACID라 하는 원자성, 일관성, 격리성, 지속성을 보장해야 한다.

### 원자성 (Atomicity)
트랜잭션 내에서 실행한 작업들은 마치 하나의 작업인 것처럼 모두 성공하든가 모두 실패해야 한다.

### 일관성 (Consistency)

모든 트랜잭션은 일관성 있는 데이터베이스 상태를 유지해야 한다. 예를 들어 데이터베이스에서 정한 무결성 제약 조건을 항상 만족해야 한다.

### 격리성 (Isolation)

동시에 실행되는 트랜잭션들이 서로에게 영향을 미치지 않도록 격리한다. 예를 들어 동시에 같은 데이터를 수정하지 못하도록 해야 한다. 격리성은 동시성과 관련된 성능 이슈로 인해 격리 수준을 선택할 수 있다.

### 지속성 (Durability)

트랜잭션을 성공적으로 끝내면 그 결과가 항상 기록되어야 한다. 중간에 시스템에 문제가 발생해도 데이터베이스 로그 등을 사용해서 성공한 트랜잭션 내용을 복구해야 한다.

## 트랜잭션 격리 수준

트랜잭션 간에 격리성을 완벽히 보장하려면 트랜잭션을 거의 차례대로 실행해야 한다. 이렇게 하면 동시성 처리 성능이 매우 나빠진다. 이런 문제로 인해 ANSI 표준은 트랜잭션의 격리 수준을 4단계로 나누어 정의했다.

트랜잭션 격리 수준은 다음과 같다.

### READ UNCOMMITTED

커밋하지 않은 데이터를 읽을 수 있다. 예를 들어 트랜잭션1이 데이터를 수정하고 있는데 커밋하지 않아도 트랜잭션 2가 수정 중인 데이터를 조회할 수 있다. 이것을 DIRTY READ라 한다. 트랜잭션 2가 DIRTY READ한 데이터를 사용하는데 트랜잭션 1을 롤백하면 데이터 정합성에 심각한 문제가 발생할 수 있다. DIRTY READ를 허용하는 격리수준을 READ UNCOMMITTED라 한다.

### READ COMMITTED

커밋한 데이터만 읽을 수 있다. 따라서 DIRTY READ가 발생하지 않는다. 하지만 NON-REPEATABLE READ는 발생할 수 있다. 예를 들어 트랜잭션 1이 회원 A를 조회 중인데 갑자기 트랜잭션 2가 회원 A를 수정하고 커밋하면 트랜잭션 1이 다시 회원 A를 조회했을 때 수정된 데이터가 조회된다. 이처럼 반복해서 같은 데이터를 읽을 수 없는 상태를 NON-REPEATABLE READ라 한다. DIRTY READ는 허용하지 않지만, NON_REPEATABLE READ는 허용하는 격리 수준을 READ COMMITTED라 한다.

### REPEATABLE READ

한 번 조회한 데이터를 반복해서 조회해도 같은 데이터가 조회된다. 하지만 PHANTOM READ는 발생할 수 있다. 예를 들어 트랜잭션1이 10살 이하의 회원을 조회했는데 트랜잭션2가 5살 회원을 추가하고 커밋하면 트랜잭션1이 다시 10살 이하의 회원을 조회했을 때 회원 하나가 추가된 상태로 조회된다. 이처럼 반복 조회 시 결과 집합이 달라지는 것을 PHANTOM READ라 한다. NON-REPEATABLE READ는 허용하지 않지만, PHANTOM READ는 허용하는 격리 수준을 REPEATABLE READ라 한다.

### SERIALIZABLE

가장 엄격한 트랜잭션 격리 수준이다. 여기서는 PHANTOM READ가 발생하지 않는다. 하지만 동시성 처리 성능이 급격히 떨어질 수 있다.

## NON-REPEATABLE READ vs. PHANTOM READ

### NON-REPEATABLE READ

A non-repeatable read occurs, when during the course of a transaction, a row is retrieved twice and the values within the row differ between reads.

### PHANTOM READ

A phantom read occurs when, in the course of a transaction, **two identical queries** are executed, and the collection of rows returned by the second query is different from the first.

### Example

- User A runs the same query twice.
- In between, User B runs a transaction and commits.
- Non-repeatable read: The A row that user A has queried has a different value the second time.
- Phantom read: All the rows in the query have the same value before and after, but different rows are being selected (because B has deleted or inserted some). Example: select sum(x) from table; will return a different result even if none of the affected rows themselves have been updated, if rows have been added or deleted.

## Note
애플리케이션 대부분은 동시성 처리가 중요하므로 데이터베이스들은 보통 READ COMMITTED 격리수준을 기본으로 사용한다. 일부 중요한 비즈니스 로직에 더 높은 격리 수준이 필요하면 데이터베이스 트랜잭션이 제공하는 잠금 기능을 사용하면 된다.

## References
자바 ORM 표준 JPA 프로그래밍 - 김영한
<http://stackoverflow.com/questions/11043712/what-is-difference-between-non-repeatable-read-and-phantom-read>