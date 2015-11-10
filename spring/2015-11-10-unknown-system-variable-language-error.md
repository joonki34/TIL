# Unknown system variable ‘language’ error

## Problem
Spring boot 프로젝트에서 mysql 쓰도록 세팅하고 실행했는데 다음과 같은 에러가 뜬다.
```
java.sql.SQLException: Unknown system variable ‘language'
```

## Solution
원인을 확인해보니 spring boot gradle 셋업이 compile('mysql:mysql-connector-java’)로 되어있었다. Spring boot 초기 설정이 이렇게 되는 듯하다. compile('mysql:mysql-connector-java:5.1.37’)로 수정하니 정상동작했다.