# spring-boot-devtools vs. spring-loaded

spring-loaded와 spring-devtools는 소스를 수정할 때마다 서버를 재시작하지 않도록 도와주는 프로젝트들이다.

## spring-loaded

spring-loaded는 JVM agent를 띄워서 코드 변경이 있을 때마다 그 코드와 관련된 class 파일을 다시 로드하는 reload 방식을 쓴다. Hot swapping이라고도 한다.

단점은 약간의 설정이 필요하다. 그리고 클래스/빈을 새로 만들거나 패키지를 옮기는 등의 작업을 할 때는 서버를 재시작해줘야 한다.

## spring-boot-devtools

설정이 필요한 spring-loaded 혹은 JRebel에 대항해 만들어진 프로젝트이다. 설정 필요없이 gradle 혹은 maven에 의존성만 추가해주면 사용이 가능하다.

spring-boot-devtools는 classloader 2개를 사용한다. 라이브러리 등에 포함되어있는 변경되지 않는 클래스들은 `base` classloader로 로드되고, 개발자의 소스는 `restart` classloader로 로드된다. 코드가 변경되면 서버가 재시작되는데, 이때 `restart` classloader만 재시작되기 때문에 일반 재시작보다 훨씬 빠르다. 이를 restart 방식이라고 하고 cool swapping이라고도 부른다.

이클립스에서는 코드를 저장할때 자동으로 재시작되지만 IntelliJ에서는 변경시마다 `Make Project`를 직접 눌러야되서 불편하다.

LiveReload 기능을 지원한다. spring-boot-devtools가 디폴트로 임베디드 LiveReload 서버를 돌리기 때문에 [livereload.com](http://livereload.com/extensions/)에서 브라우저 확장 플러그인을 설치하면 새로고침을 누를 필요 없이 알아서 변경된다.

## Conclusion

spring-boot-devtools는 의존성 추가 외에는 설정이 필요하지않고 LiveReload가 지원되는 것이 장점이지만 IntelliJ에서는 매번 `Make Project`를 해줘야 한다. 둘다 장단점이 있으므로 비교해서 마음에 드는 걸로 쓰는 것이 바람직할 것 같다.

## References
<https://spring.io/blog/2015/06/17/devtools-in-spring-boot-1-3>  
<http://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html>  
<https://dzone.com/articles/spring-boot-developer-tools-in-130>