# Glossaries

## General

### UTF-8 vs. Unicode
- Unicode: 컴퓨터에서 세계 각국의 언어를 통일된 방법으로 표현할 수 있게 제안된 국제적인 문자 코드 규약  
- UTF-8: 유니코드를 위한 가변 길이 문자 인코딩 방식 중 하나. Sequences of bytes를 sequences of characters로 변환하는 방법 중 하나.

<http://stackoverflow.com/questions/643694/utf-8-vs-unicode/643713#643713>

### Static typing vs. Dynamic typing
- Static typing: 컴파일 타임에 type checking하는 방식 (e.g. Java, C, C++)
- Dynamic typing: 런타임에 type checking하는 방식 (e.g. python, javascript)

<http://stackoverflow.com/questions/1517582/what-is-the-difference-between-statically-typed-and-dynamically-typed-languages/1520342#1520342>

### SSL
절차는 다음과 같다.

1. 나는 암호문을 주고 받을 상대에게 접속 요청을 한다.
2. 상대는 자신의 공개키를 나에게 보낸다.
3. 나는 암호화할 때 사용할 대칭키를 상대의 공개키로 암호화해서 보낸다.
4. 상대는 내가 보낸 암호문을 자신의 개인키로 풀어 대칭키를 알아낸다.
5. 대칭키를 사용해 서로 암호문을 주고 받는다.

내가 접속한 사이트가 진짜인지 알기 위해 제3의 인증기관을 거친다. 절차는 다음과 같다.

1. 인터넷 사이트는 자신의 정보와 공개키를 인증기관에 제출한다.
2. 인증기관은 검증을 거친 후 사이트 정보와 공개키를 인증기관의 개인키로 암호화한다. 이것이 사이트 인증서이다.
3. 인증기관은 웹 브라우저에게 자신의 공개키를 제공한다.
4. 사용자가 웹 브라우저로 사이트에 접속하면 사이트는 자신의 인증서를 웹 브라우저에게 보낸다.
5. 웹 브라우저는 인증 기관의 공개키로 서버 인증서를 해독하여 검증한다.
6. 이렇게 얻은 사이트 공개키로 대칭키를 암호화해서 보낸다.
7. 사이트는 자신의 개인키로 암호문을 해독해서 대칭키를 얻고 암호문을 주고 받는다.

<http://likelink.co.kr/14005>

### Anti-aliasing
이미지의 가장자리를 흐리게 만들어서 더 부드럽게 보여주는 방식

### Authentication vs. Authorization
- Authentication: A가 본인이 A라고 주장하는걸 확인하는 작업 (login + password)
- Authorization: A가 특정 작업을 하는게 허용되는지 확인하는 작업 (permissions)

<http://stackoverflow.com/questions/6556522/authentication-versus-authorization>

### Shim vs. Polyfill
- Shim: 새로운 API를 오래된 환경에서 쓸 수 있도록 하는 라이브러리
- Polyfill: Shim의 일종으로 웹 기술과 관련됨.

### RESTful API
Representational state transfer. HTTP Method. SOAP 대체

대표적 특징

- Client-server: client(browser)와 서버 분리
- Stateless: 무상태성. 서버가 상태를 저장하지 않음
- Cacheable
- Self-descriptiveness: API 내용만 보고도 쉽게 이해 가능

### SOAP
클라이언트와 웹 서버간 메시지 전송 시에 사용하는 기술. XML과 HTTP를 기본으로 함.

### Flash of unstyled content
브라우저로 웹문서에 접근했을때 CSS 스타일 적용되지 않은 상태에서 화면이 표시되어 화면 깜빡임을 유발하는 현상.

SPA에서는 js가 DOM를 그리기 때문에 이 현상이 더 두드러지게 나타남. Server-side rendering로 해결할 수 있음. 

### JSONP

`<script>` 태그를 이용한 same-origin policy 우회법. 서버에서도 지원해줘야 함.

참고: <http://stackoverflow.com/a/2067584>

### Security through obscrutiny
은닉을 통한 보안. 보안 방식을 공개하지 않음으로써 보안을 강화하는 방식. 좋은 방식으로 여기지지 않음.

### Framework vs. Library
- Framework: 프레임워크가 내 코드를 call함. Inversion of Control
- Library: 내가 라이브러리를 사용함. A collection of functionality that you can call.

### Canonical form
A unique representation for every object.

### Recursion
- Advantages: 특정 알고리즘에 대한 구현이 쉬워진다 (e.g. tree traversal). Time complexity를 줄이기도 한다.
- Disadvantages: 대부분의 경우에 느리다. 스택을 많이 차지한다. 

##### Tail Recursion
Recursion할 때 이전의 결과값을 다음 recursive step에 전달하는 방식. 일반적인 recursion과는 다르게 stack을 더 쓰지 않는다.

<http://stackoverflow.com/questions/33923/what-is-tail-recursion>

### i18n vs. L10n
- i18n: 소프트웨어를 여러 나라 언어와 문화를 지원하기 쉬운 구조로 만드는 것
- L10n: 소프트웨어를 특정 지역의 언어와 문화를 지원하도록 만드는 것이다. 예를 들면 중국어나 일본어를 지원하는 것.

### MVC
- Controller: The Controller's job is to translate incoming requests into outgoing responses. In order to do this, the controller must take request data and pass it into the Service layer.
- View: The View's job is to translate data into a visual rendering for response to the Client (ie. web browser or other consumer).
- Model: The Model's job is to represent the problem domain, maintain state, and provide methods for accessing and mutating the state of the application.

<http://www.bennadel.com/blog/2379-a-better-understanding-of-mvc-model-view-controller-thanks-to-steven-neiland.htm>

### Open-closed principle
The Open Close Principle states that the design and writing of the code should be done in a way that new functionality should be added with minimum changes in the existing code. The design should be done in a way to allow the adding of new functionality as new classes, keeping as much as possible existing code unchanged.

<http://www.oodesign.com/open-close-principle.html>

### Blue Green Deployment
한 머신에 웹 서버를 두개 띄워놓은 후 router가 둘 중 하나를 바라보게 함. 다음 버전을 배포할 때는 현재 idle 상태인 웹서버에 배포하고 router가 해당 서버를 바라보게 하도록 바꿔줌.

### Imperative vs. Declarative Programming
You arrive at Red Lobster, approach the front desk and say…

An imperative approach (HOW): I see that table located under the Gone Fishin’ sign is empty. My husband and I are going to walk over there and sit down.  
A declarative approach (WHAT): Table for two, please.

The imperative approach is concerned with HOW you’re actually going to get a seat. You need to list out the steps to be able to show HOW you’re going to get a table. The declarative approach is more concerned with WHAT you want, a table for two.

### Domain driven design
Domain: 해결하고자 하는 문제

So the Domain is the world of the business, the Model is your solution and the Domain Model is the structured knowledge of the problem.

## HTTP

### CORS
Cross-origin resource sharing. Same-origin policy는 document 혹은 script가 다른 origin의 resource에 접근하는 것을 막는 정책. JSONP 혹은 CORS로 해결할 수 있는데 CORS를 권장.

CORS 통신 절차

- OPTIONS로 해당 서버에 CORS 요청 가능한지 확인
- 가능하면 실제 request 요청
- 서버로부터 응답 수신 (Access-Control-Allow-Origin 같이 옴)

<http://www.html5rocks.com/en/tutorials/cors/#toc-handling-a-not-so-simple-request>

### Session vs. Cookie
Sessions are server-side files that contain user information, while Cookies are client-side files that contain user information. Sessions have a unique identifier that maps them to specific users. This identifier can be passed in the URL or saved into a session cookie.

Session cookie: a cookie that is erased when the user closes the Web browser.

### URL Encoding
Since URLs often contain characters outside the ASCII set, the URL has to be converted into a valid ASCII format.

URL encoding replaces unsafe ASCII characters with a "%" followed by two hexadecimal digits.

### SNI

Server Name Indication의 약자. HTTPS를 사용할 때 하나의 서버(IP)에서 여러 호스트 이름 또는 도메인 이름(사람이 읽을 수 있는 웹 사이트 이름)을 지원할 수 있도록 지원하는 TLS 확장. SNI는 클라이언트 Hello 메시지 또는 TLS 핸드셰이크의 첫 번째 단계에 호스트 이름을 포함한다.

<https://www.cloudflare.com/ko-kr/learning/ssl/what-is-sni/>

## HTML

### Layout Thrashing
Whenever the DOM is written to, the current layout is ‘invalidated,’ and will need to be reflowed. The browser usually waits to do this until the end of the current operation or frame. However, if we ask (via JavaScript) for geometric values before the current operation or frame is complete, the browser is forced to reflow the layout immediately. This is known as a ‘forced synchronous layout,’ and has the potential to be devastating to performance.

<https://afasterweb.com/2015/10/05/how-to-thrash-your-layout/>

#### Reflow
A reflow computes the layout of the page. A reflow on an element recomputes the dimensions and position of the element, and it also triggers further reflows on that element’s children, ancestors and elements that appear after it in the DOM. Then it calls a final repaint. Reflowing is very expensive, but unfortunately it can be triggered easily.

## CSS

## Javascript

## React
### Virtual DOM
DOM is an in-memory representation of this text. The DOM trees are huge nowadays. Since we are more and more pushed towards dynamic web apps (SPAs), we need to modify the DOM tree incessantly and a lot. And this is a real performance and development pain.

The Virtual DOM is an abstraction of the HTML DOM. It is lightweight and detached from the browser-specific implementation details.

There’s no big difference between the “regular” DOM and the virtual DOM. You can think of the virtual DOM like a blueprint.

React creates a tree of custom objects representing a part of the DOM. For example, instead of creating an actual DIV element containing a UL element, it creates a React.div object that contains a React.ul object. It can manipulate these objects very quickly without actually touching the real DOM or going through the DOM API. Then, when it renders a component, it uses this virtual DOM to figure out what it needs to do with the real DOM to get the two trees to match.

## Java

### Polymorphism

The dictionary definition of polymorphism refers to a principle in biology in which an organism or species can have many different forms or stages.

Subclasses of a class can define their own unique behaviors and yet share some of the same functionality of the parent class.

The Java virtual machine (JVM) calls the appropriate method for the object that is referred to in each variable. It does not call the method that is defined by the variable's type. This behavior is referred to as virtual method invocation and demonstrates an aspect of the important polymorphism features in the Java language.

<https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html>

객체지향개념에서 다형성이란 '여러 가지 형태를 가질 수 있는 능력'을 의미하며 자바에서는 한 타입의 참조변수로 여러 타입의 객체를 참조할 수 있도록함으로써 다형성을 프로그램적으로 구현하였다.

### hashCode() and equals()
hashCode()는 HashTable/HashSet/HashMap에 쓰이는 key.  

HashTable에서 특정 entry를 찾을 때 hash code를 참조해서 해당 영역(e.g. list, bucket)을 참조함. 하지만 해당 영역에 hash code는 같지만 서로 다른 객체들이 존재할 수 있다. 이때 equals()를 사용하게 된다. 

이렇기 때문에 보통 equals()를 override할 때는 hashCode()도 override해야한다.

hashCode()를 override할 때 원칙은 다음과 같다.

- object1과 object2가 equals()를 통해 equal하면 둘은 반드시 hash code가 같아야 한다.
- object1과 object2의 hash code가 같아도 equal할 필요는 없다.

참고: <http://tutorials.jenkov.com/java-collections/hashcode-equals.html>

### Checked and unchecked exception
- Checked exception: 컴파일 타임에 체크되는 exception. 어떠한 method가 checked exception을 throw하면 반드시 try-catch block 혹은 throws keyword로 처리를 해야한다. 처리하지 않으면 컴파일 에러를 뱉는다.
- Unchecked exception: 컴파일 타임에 체크되지 않는 exception. 프로그램이 unchecked exception를 throw한다면 해당 exception을 handle하지 않아도 컴파일 에러를 뱉지 않는다. 모든 unchecked exception은 RuntimeException의 subclass이다.

<http://beginnersbook.com/2013/04/java-checked-unchecked-exceptions-with-examples/>

### Comparable vs. Comparator
Implementing `Comparable` means "I can compare myself with another object." This is typically useful when there's a single natural default comparison.

Implementing `Comparator` means "I can compare two other objects." This is typically useful when there are multiple ways of comparing two instances of a type - e.g. you could compare people by age, name etc.

<http://www.digizol.com/2008/07/java-sorting-comparator-vs-comparable.html>

### Access Level
|Modifier|Class|Package|Subclass|World|
|---|---|---|---|---|
|public|Y|Y|Y|Y|
|protected|Y|Y|Y|N|
|no modifier|Y|Y|N|N|
|private|Y|N|N|N|

### serialVersionUID
The serialVersionUID is used as a version control in a Serializable class. If you do not explicitly declare a serialVersionUID, JVM will do it for you automatically, based on various aspects of your Serializable class, as described in the Java(TM) Object Serialization Specification.

The serialVersionUID have to match during the serialization and deserialization process.

When your serialization class is updated with some incompatible Java type changes to a serializable class, you have to update your serialVersionUID.

If no serialVersionUID is declared, JVM will use its own algorithm to generate a default SerialVersionUID.

The default serialVersionUID computation is highly sensitive to class details and may vary from different JVM implementation, and result in an unexpected InvalidClassExceptions during the deserialization process.

<http://stackoverflow.com/a/888378>

### String literal "abc" vs. new String("abc")
String literal은 String pool(intern pool)에서 관리됨. new String()을 쓰면 object가 새로 생성되어 heap에서 관리됨. new String()은 쓸데없는 object를 한번 더 생성하므로 지양해야 함. 다음과 같이 ==를 쓸 경우 결과가 달라짐.

```java
"abc" == "abc" // true
new String("abc") == new String("abc") // false
```

<http://stackoverflow.com/a/3297951>
<http://stackoverflow.com/a/21208178>

### Vector vs. ArrayList
Vector and ArrayList are almost equivalent but Vector synchronizes on each individual operation.

하지만 보통 multi-threading 환경에서 단일 operation에 대한 synchronization은 필요 없고 성능 저하를 야기할 뿐임. 둘 다 synchronized해줘야되는건 마찬가지.

Vector보다는 synchronizedList() 쓰는 것이 좋음.

StringBuffer로 같은 이유에서 쓰지 않는 것이 좋음.

Vector vs. synchronizedList(): <http://stackoverflow.com/a/14932111>

### Servlet container thread allocation
Servlet containers are thread per request. One thread serves the whole request. 

Thread per connection would be the same thing except the thread is used for an entire connection, which could be multiple requests and could also have a lot of dead time in between requests. 

<http://stackoverflow.com/questions/7457190/how-are-threads-allocated-to-handle-servlet-request>

### Garbage Collection
stop-the-world: GC을 실행하기 위해 JVM이 어플리케이션 실행을 멈추는 것. stop-the world가 발생하면 GC를 실행하는 쓰레드를 제외한 모든 쓰레드가 작업을 멈춘다.

GC 튜닝: stop-the-world 시간을 줄이는 것.

가비지 컬렉터는 두가지 가설 아래 만들어졌다.

- 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

JVM에서는 크게 2개의 물리적 공간이 있다.

- Young 영역: 새롭게 생성한 객체의 대부분이 위치하는 영역. 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 많은 객체가 Young 영역에 생성되었다가 사라진다. 이 영역에서 객체가 사라질 때 Minor GC라고 한다.
- Old 영역: 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사된다. 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다. 이 영역에서 객체가 사라질 때 Major GC(혹은 Full GC)가 발생한다고 말한다.

#### Young 영역
Young 영역은 Eden 영역과 2개의 Survivor 영역, 총 3개의 영역으로 나뉜다. 절차는 다음과 같다.

- 새로 생성한 대부분의 객체는 Eden 영역에 위치한다.
- Eden 영역에서 GC가 한번 발생한 후 살아남은 객체들은 Survivor 영역 중 하나로 이동된다.
- Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓인다.
- 하나의 Survivor 영역이 가득차게 되면, 그 중에서 살아남은 객체를 다른 Survivor 영역으로 이동한다. 그리고 가득찬 Survivor 영역은 아무 데이터도 없는 상태로 된다.
- 이 과정을 반복하다가 계속 살아남아있는 객체는 Old 영역으로 이동하게 된다.

#### Old 영역
Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행한다. GC 방식에서 처리 절차가 다 다르다.

#####G1(Garbage First) GC
G1 GC는 바둑판의 각 영역에 객체를 할당하고 GC를 실행한다. 그러다가, 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행한다. 즉, Young의 세가지 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식이라고 이해하면 된다. 기존에 있던 어떤 GC보다도 빠르다.

참고: <http://d2.naver.com/helloworld/1329>


### Weak Reference
WeakReference로 객체를 감쌌을 경우, 해당 객체에 reachable한 객체가 WeakReference 뿐이면 다음 GC cycle에 GC됨. SoftReference는 WeakReference과 같지만 잔여 메모리가 남아있을 때까지 GC되지 않음.

### Can you override static method in Java?
A user cannot override static methods in Java, because method overriding is based upon dynamic binding at runtime and static methods are statically binded at compile time. 

### What is the difference between processes and threads?
A process is an execution of a program, while a Thread is a single execution sequence within a process. A process can contain multiple threads. A Thread is sometimes called a lightweight process.

Both processes and threads are independent sequences of execution. The typical difference is that threads (of the same process) run in a shared memory space, while processes run in separate memory spaces.

### What is an Iterator?
The Iterator interface provides a number of methods that are able to iterate over any Collection. Each Java Collection contains the iterator method that returns an Iterator instance. **Iterators are capable of removing elements from the underlying collection during the iteration.**

### What is PriorityQueue?
The PriorityQueue is an unbounded queue, based on a priority heap and its elements are ordered in their natural order. At the time of its creation, we can provide a Comparator that is responsible for ordering the elements of the PriorityQueue.

PriorityQueue provides O(log(n)) time for the enqueing and dequeing methods (offer, poll, remove() and add); linear time for the remove(Object) and contains(Object) methods; and constant time for the retrieval methods (peek, element, and size).

### HashSet vs. TreeSet
The HashSet is Implemented using a hash table and thus, its elements are not ordered. The add, remove, and contains methods of a HashSet have constant time complexity O(1). On the other hand, a TreeSet is implemented using a tree structure. The elements in a TreeSet are sorted, and thus, the add, remove, and contains methods have time complexity of O(logn).

### What does System.gc() and Runtime.gc() methods do?
These methods can be used as a hint to the JVM, in order to start a garbage collection. However, this it is up to the Java Virtual Machine (JVM) to start the garbage collection immediately or later in time.

### When is the finalize() called ? What is the purpose of finalization?

The finalize method is called by the garbage collector, just before releasing the object’s memory. It is normally advised to release resources held by the object inside the finalize method.

### PermGen space

The permanent generation is special because it holds meta-data describing user classes (classes that are not part of the Java language).

Examples of such meta-data are objects describing classes and methods and they are stored in the Permanent Generation.

Applications with large code-base can quickly fill up this segment of the heap which will cause `java.lang.OutOfMemoryError`: PermGen no matter how high your -Xmx and how much memory you have on the machine.

### What is a Servlet?

The servlet is a Java programming language class used to process client requests and generate dynamic web content.

### Static vs. non-static inner class

A non-static nested class has full access to the members of the class within which it is nested. A static nested class does not have a reference to a nesting instance, so a static nested class cannot invoke non-static methods or access non-static fields of an instance of the class within which it is nested.

## Spring

### Bean
1) The objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container.

2) Spring beans are just object instances that are managed by the Spring container, namely, they are created and wired by the framework and put into a "bag of objects" (the container) from where you can get them later.

### Dependency Injection
DI는 IoC의 한 종류.

It is a process whereby objects define their dependencies, that is, the other objects they work with, only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The IoC container then injects those dependencies when it creates the bean.

### AOP
- Advice: 언제 공통 관심기능을 핵심 로직에 적용할 지를 정의. (e.g. @Before, @After)
- Join point: Advice를 적용 가능한 지점을 의미. In Spring AOP, a join point always represents a method execution.
- Pointcut: A predicate that matches join points. Pointcut are expressions that is matched with join points to determine whether advice needs to be executed or not.
- Weaving: Advice를 핵심 로직 코드에 적용하는 과정.
- Aspact: 여러 객체에 공통으로 적용되는 공통관심 사항

```java
@Before("filteredTraceMethodsInDemoPackage()")
public void beforeTraceMethods(JoinPoint joinPoint) {
    // trace logic ..
}
```

여기에서 @Before가 advice, filteredTraceMethodsInDemoPackage()가 pointcut이다.

### fixedDelay vs. fixedRate
- fixedDelay: 이전 수행이 종료된 시점부터 delay 후에 재호출 (long-polling에 사용하기도 함.)
- fixedRate: 이전 수행이 시작된 시점부터 delay 후에 재호출 (동시에 여러개 돌고 있는 가능성 있음)

### `@Bean` vs. `@Component`

`@Bean`: 개발자가 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 등록하고 싶은 경우 사용 (e.g. `ObjectMapper`의 경우 `ObjectMapper` 클래스에 `@Component`를 선언할 수 없으니 `ObjectMapper`의 인스턴스를 생성하는 메소드를 만들고 해당 메소드에 `@Bean`을 선언하여 Bean으로 등록한다.)

```java
@Bean
public ObjectMapper objectMapper() {
  return new ObjectMapper();
}
```

`@Component`: 개발자가 직접 컨트롤이 가능한 Class들의 경우에 사용

그럼 개발자가 생성한 Class에 `@Bean`은 선언이 가능할까? 정답은 No 이다. `@Bean`과 `@Component`는 각자 선언할 수 있는 타입이 정해져있어 해당 용도외에는 컴파일 에러를 발생시킨다.

### 테스트 어노테이션 정리

- `@WebAppConfiguration`: 테스트 시 `ApplicationContext`가 아닌 `WebApplicationContext`를 로드하도록 명시하는 어노테이션
- `@IntegrationTest`: integration test를 위해 웹 어플리케이션을 fully load하도록 하는 어노테이션. 테스트 시 `TestRestTemplate` 등을 이용해 로드해놓은 어플리케이션에 request를 보내는 방식으로 테스트 할 수 있다.
- `@WebIntegrationTest`: `@WebAppConfiguration` + `@IntegrationTest`
- `@ContextConfiguration`: WebApplicationContext를 로드할 때 어떤 `@Configuration`과 컴포넌트를 로드할지 지정할 수 있는 어노테이션.
- `@SpringApplicationConfiguration`: Spring Boot용 `@ContextConfiguration`. `@SpringBootTest` 등장으로 인해 deprecated.
- `@SpringBootTest`: 위의 기능이 전부 들어있는 어노테이션. Spring Boot 1.4.0부터 사용 가능. [참고](http://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/context/SpringBootTest.html)

## Data Structure

### Rate Limiting

- Leaky Bucket
  - 장점: 고정된 처리율을 갖고 있어서 안정적 출력이 필요한 경우에 적합하다.
  - 단점: 버스트 트래픽을 처리하지 못한다. 처리 속도가 늦으면 기존 요청을 처리하느라 새로운 요청들이 처리되지 못한다.
- Token Bucket
  - 장점: 버스트 트래픽을 처리할 수 있다. 구현이 쉽고 효율적이다. 
  - 단점: 파라미터를 2개 조정해야 한다 (refill rate, bucket size)
- Fixed Window
  - 장점: 가장 간단하고 메모리 효율이 좋다.
  - 단점: 윈도우 경계 시점에 트래픽 스파이크를 허용함
- Sliding Window Log
  - 요청의 타임스탬프를 추적한다. 만료된 타임스탬프는 제거한다.
  - 장점: 정교하다. 
  - 단점: 거부된 요청의 타임스탬프도 보관하기 때문에 다량의 메모리를 사용한다.
- Sliding Window Counter
  - Fixed Window + Sliding Window Log
  - 장점: 가장 정확한 편이다. Sliding Window Log보다는 메모리 효율적이다.
  - 단점: 구현하기 복잡하다. 이전 윈도우의 데이터가 갖고 있어야 함.
- 참고: 가상 면접 사례로 배우는 대규머 시스템 설계 기초

<https://etloveguitar.tistory.com/128>

### Bloom Filter

### Consistent Hashing