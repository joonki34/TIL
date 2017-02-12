# Interpreter vs. JIT vs. AOT

Interpreter, JIT, AOT를 자바 기준으로 설명한다.

## Interpreter

자바에서 인터프리터는 바이트코드 명령어를 하나씩 읽어서 해석하고 실행한다. 하나씩 해석하고 실행하기 때문에 바이트코드 하나하나의 해석은 빠른 대신 인터프리팅 결과의 실행은 느리다는 단점을 가지고 있다. 즉, 바이트코드라는 '언어'는 기본적으로 인터프리터 방식으로 동작한다.

## JIT(Just-In-Time) 컴파일러

인터프리터의 단점을 보완하기 위해 도입된 것이 JIT 컴파일러이다. 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 네이티브 코드로 변경하고, 이후에는 해당 메서드를 더 이상 인터프리팅하지 않고 네이티브 코드로 직접 실행하는 방식이다. 네이티브 코드를 실행하는 것이 하나씩 인터프리팅하는 것보다 빠르고, 네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일된 코드는 계속 빠르게 수행되게 된다.

JIT 컴파일러가 컴파일하는 과정은 바이트코드를 하나씩 인터프리팅하는 것보다 훨씬 오래 걸리므로, 만약 한 번만 실행되는 코드라면 컴파일하지 않고 인터프리팅하는 것이 훨씬 유리하다. 따라서, JIT 컴파일러를 사용하는 JVM들은 내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크하고, 일정 정도를 넘을 때에만 컴파일을 수행한다.

JIT 컴파일러는 바이트코드를 일단 중간 단계의 표현인 IR(Intermediate Representation)로 변환하여 최적화를 수행하고 그 다음에 네이티브 코드를 생성한다.

Just-in-time compilation is the conversion of non-native code, for example bytecode, into native code just before it is executed.

## JIT vs. AOT(Ahead-Of-Time)

JIT compiler compiles the program as it is running, an AOT compiler compiles the program before it is running.

### In Android

JIT는 실행 시점에 소소코드를 번역한다. 설치는 빠르지만 실행 시점에 느리게 된다. 번역한 정보를 메모리에 올려야 하기 때문에 메모리를 많이 먹는다. 

AOT는 설치 시점에 소스코드를 번역한다. 설치가 느리고, 번역을 해서 따로 파일을 저장하기 때문에 용량을 많이 먹게 된다. 하지만 실행 시점에 미리 번역한 파일을 실행하므로 빠르게 실행이 가능하다. 

### In Angular

With JIT, you compile the app in the browser, at runtime, as the application loads.

With AOT, the browser loads code of the application that has been pre-compiled at build time. It means the application can render immediately.

## Resources

<http://d2.naver.com/helloworld/1230>  
<http://118k.tistory.com/250>  
<http://dev.apollodata.com/angular2/ahead-of-time.html>