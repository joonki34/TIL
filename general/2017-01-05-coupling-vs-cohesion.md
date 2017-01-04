# Coupling vs. Cohesion

결합도는 낮을수록, 응집도는 높을수록 이상적인 모듈화라고 한다.

## 결합도 (Coupling)

결합도는 모듈과 모듈간의 상호 의존 정도를 말한다.

As for coupling, it refers to how related are two classes / modules and how dependent they are on each other. Being low coupling would mean that changing something major in one class should not affect the other. High coupling would make your code difficult to make changes as well as to maintain it, as classes are coupled closely together, making a change could mean an entire system revamp.

## 응집도 (Cohesion)

응집도는 모듈 내부의 기능적인 집중 정도를 말한다. 다르게 말해, 하나의 모듈의 내부 구성 요소 간의 기능적 관련성을 의미한다.

Cohesion refers to what the class (or module) will do. Low cohesion would mean that the class does a great variety of actions and is not focused on what it should do. High cohesion would then mean that the class is focused on what it should be doing, i.e. only methods relating to the intention of the class.

## References
<http://stackoverflow.com/questions/3085285/cohesion-coupling>