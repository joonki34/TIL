# Domain-driven Design

## Domain

The domain is the world of the business you are working with and the problems they want to solve.

## Model

The Model of an Domain Driven Designed project is your solution to the problem.

The Model is system of abstractions that describes selected aspects of a domain and can be used to solve problems related to that domain.

This means your Model should be focused knowledge around a specific problem that is simplified and structured to provide a solution.

## Domain Model

The Domain Model is not a particular diagram.

The Domain Model is your organised and structured knowledge of the problem.

## Ubiquitous Language

A language structured around the domain model and used by all team members to connect all the activities of the team with the software.

Use the domain model as the backbone of a language.

Use the ubiquitous language in all communication within team and in the code.

Use the same language in diagrams, writing, and especially speech.

## Entity

- 모델을 표현
- 고유의 식별 값을 가짐
- 모델과 관련된 비즈니스 로직을 수행

## Aggregate

- 관련된 객체들의 묶음
- 데이터 변경 시 한 단위로 처리됨
- Aggregate는 경계를 가짐
  - 기본적으로, 한 Aggregate에 속한 객체는 다른 Aggregate에 속하지 않음

### Aggregate Root
- Aggregate의 내부 객체들을 관리
- Aggregate 외부에서 접근할 수 있는 유일한 객체

## Repository

- Entity를 보관하는 장소
- 기본 규칙
  - Aggregate 당 한 개의 Repository 인터페이스
  - Aggregate의 CRUD 기본 단위는 루트

## Specification

Repository에서 Entity를 검색하기 위한 필터

## Service

한 Entity나 Aggregate에 속하지 않은 도메인 기능을 도메인 서비스로 분리

## References
<http://www.slideshare.net/madvirus/ddd-final?next_slideshow=1>
<http://www.slideshare.net/baejjae93/domain-driven-design-37608771>
<http://culttt.com/2014/11/12/domain-model-domain-driven-design/>