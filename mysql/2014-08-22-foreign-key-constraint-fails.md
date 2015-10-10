# Foreign Key Contraint Fails

## Problem
테이블 A, B가 있고 B가 A를 참조하고 있다고 치자. B는 truncate 혹은 레코드 전체 삭제가 가능하다. 하지만 A는 B가 참조하고 있다고 이 작업을 할 수 없다.

Error Message: Cannot delete or update a parent row a foreign key constraint fails

## Trial 1

truncate할 때 foreign key contstraint를 체크 안하게 만들고 truncate 후에 다시 체크하게 함.

```
SET FOREIGN_KEY_CHECKS=0;
truncate A;
SET FOREIGN_KEY_CHECKS=1;
```

이 방법의 문제는 MySQL에서 실행하는데에는 문제 없지만 myBatis의 xml에서 저 명령어를 못 알아먹음. 이상한 건 delete 태그인데 truncate을 알아들음.

고로 지금 환경에서 못 써먹음. (Hibernate에서는 될 수도 있다는 말을 들음)

## Trial 2

delete로 데이터 모두 삭제하고 auto increment value를 1로 바꿔줌.
truncate을 쓰는 이유? auto increment value 리셋하려고.
그래서 delete 후에 값 바꿔주는 형태로 truncate하는 것처럼 만들 수 있음.

```
delete from A // 전체 삭제
ALTER TABLE A AUTO_INCREMENT=1;
```

문제는? myBatis가 이것도 못 알아먹음.

## Solution

개발 중인 어플리케이션에서 이 기능이 필요하기 때문에 피할 수 없음.

결국 한 것?  
그냥 foreign key를 다 지워버림. 무결성은 그냥 서버단에서 체크하기로 함. 읽기 작업을 더 많이 하기 때문에 MyISAM으로 넘어갈까도 생각했지만 안정성이 떨어진다는 충고가 있어 보류.  
하지만 지금 개발하는 시스템(scrm)이 읽기 작업을 많이 하는 편이고 myBatis가 트랜잭션 처리를 해줘서 MyISAM이 더 나을 것 같다는 생각이 듬.
사실 이미 실서버에는 MyISAM으로 올라가있어 나중에 일이 터지면 생각해볼 예정.