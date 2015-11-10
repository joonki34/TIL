# Set up profile-specific application.properties

개발 환경, QA 환경, 서비스 환경에 따라 DB 정보 등의 설정이 달라져야 하는 경우가 많다. Spring Boot는 application.properties 명을 아래와 같이 지정함으로써 profile 기능을 제공한다.
```
application-{profile명}.properties
```

이 때, application.properties에서 아래의 property에 현재 적용을 원하는 profile명을 명시한다.  
```
spring.profiles.active
```

기존에 존재하는 application.properties는 기본적으로 모든 profile에 포함이 되게 된다. (super class격)

## References
<http://m.blog.naver.com/genesis1343/220490910265>