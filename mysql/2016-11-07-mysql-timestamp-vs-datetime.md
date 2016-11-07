# TIMESTAMP vs. DATETIME in MySQL

TIMESTAMP와 DATETIME은 둘다 `2015-11-07 20:30:00` 형태의 값을 저장하는 듯 보인다. 하지만 차이점이 있다.

## TIMESTAMP

TIMESTAMP는 해당 record의 데이터가 바뀔 때마다 그 당시의 시간을 기록해놓는 용도로 주로 사용한다. `ON UPDATE CURRENT_TIMESTAMP` 등의 옵션을 이용하면 된다.

MySQL 5부터 TIMESTAMP 값은 time zone의 영향을 받고 UTC로 변환하여 저장된다.

그래서 `set time_zone="america/new_york";` 등을 이용해 DB의 time zone을 바꿔버리면 TIMESTAMP 값이 바뀐다.

TIMESTAMP는 4바이트라서 `2038-01-19 03:14:07 UTC`까지의 정보 밖에 저장하지 못해서 신중히 써야할 필요가 있다. 또한, 날짜가 1970년부터 시작하므로 주의해야한다. 대신 인덱싱이 빨리 되는 등 성능상의 이점이 있다.

## DATETIME

DATETIME은 TIMESTAMP와 달리 time zone의 영향을 받지 않는다. 또한 8바이트를 사용하기 때문에 날짜 범위를 걱정할 필요가 없다. 정확히는 `1000-01-01 00:00:00`에서 `9999-12-31 23:59:59`까지 지원한다.

## 결론

다수의 time zone을 고려해야하는 서비스가 아니라면 DATETIME을 써도 무방할 것이다. 그렇지 않을 때는 TIMESTAMP를 고려해야하지만 2038년이 되면 문제가 발생할 소지가 충분하다. (참고: [2038년 문제](https://en.wikipedia.org/wiki/Year_2038_problem))

이를 해결하려면 어플리케이션단에서 직접 UTC로 변환하여 저장하는 등의 수고가 필요할 것 같다.

## References
<http://stackoverflow.com/questions/409286/should-i-use-field-datetime-or-timestamp>
<http://stackoverflow.com/questions/12882663/mysql-is-not-allowing-on-update-current-timestamp-for-a-datetime-field>
Timezone 바꿨을 때의 차이: <http://stackoverflow.com/a/7137276>