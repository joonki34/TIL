# Classloader

C/C++에서는 소스코드 파일들을 컴파일하여 object file(machine-specific instruction)으로 변환하고 이를 실행 파일(executable file)로 만든다. object file들을 묶어서 실행 파일로 만드는 작업을 linking이라고 한다.

자바는 동적으로 컴파일되는 언어(dynamically compiled language)이고, 자바 컴파일러에 의해 생성된 .class 파일들은 JVM이 필요할때 로드한다. 다시 말해, 자바에서는 linking process가 JVM에 의해 동적으로 실행된다.

이러한 클래스 로딩은 JVM에 있는 클래스로더(`java.lang.ClassLoader`)에 의해 이루어진다. 개발자는 `java.lang.ClassLoader`를 상속받아 커스텀 클래스로더를 구현할 수 있다.

클래스로더를 구현할때 부모 클래스로더를 참조해야되는데, 최상위 클래스로더는 bootstrap classloader이다. Bootstrap classloader는 코어 자바 라이브러리를 로드하는 클래스 로드이다.

예를 들어, 자바 어플리케이션이 실행될때 처음 실행되는 함수는 보통 main()이다. main()이 있는 클래스는 다른 클래스들을 참조하기 때문에 클래스로더가 참조된 모든 클래스를 로드하는 시작점이 된다.

## TODO
1. Bootstrap classloader vs. System classloader
2. 개발자가 만든 클래스는 어떤 classloader가 로드하는건지 모르겠음.

## References
<https://en.wikipedia.org/wiki/Java_Classloader>  
<http://www.cprogramming.com/compilingandlinking.html>  
<http://stackoverflow.com/questions/2424604/what-is-a-java-classloader>