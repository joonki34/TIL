# What is @FunctionalInterface?

공식 문서 스펙이 가장 정확하다. 스펙에 따르면,

`@FuncionalInterface`는 특정 interface가 functional interface임을 명시하기 위한 어노테이션 타입이다. 

Functional interface는 반드시 하나의 abstract method만 가지고 있어야한다. Default method는 implementation이므로 abstract method가 아니다.

여기서 주의할 점은, 모든 interface 구현체(class)는 java.lang.Object를 어딘가에서든 상속받을 것이기 때문에, interface에서 java.lang.Object의 public method을 상속하는 abstract method를 선언해도 functional interface의 abstract method count에는 포함되지 않는다. (e.g. `equals()`)

예를 들어, interface에서 임의의 abstract method인 `hello()`와 java.lang.Object 메소드인 `equals()`를 선언해도 abstract method count는 1이다.

>An informative annotation type used to indicate that an interface type declaration is intended to be a functional interface as defined by the Java Language Specification. Conceptually, a functional interface has exactly one abstract method. Since default methods have an implementation, they are not abstract. If an interface declares an abstract method overriding one of the public methods of java.lang.Object, that also does not count toward the interface's abstract method count since any implementation of the interface will have an implementation from java.lang.Object or elsewhere.

## References

<https://docs.oracle.com/javase/8/docs/api/java/lang/FunctionalInterface.html>
<http://stackoverflow.com/questions/14655913/precise-definition-of-functional-interface-in-java-8>