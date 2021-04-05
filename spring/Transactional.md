# @Transactional 어노테이션 

<br>

## 트랜잭션 데이터 공유 이슈

#### Dirty Read
트랜잭션 A가 어떤 값을 변경하고 커밋하기 전일떄, 트랜잭션 B가 같은 값을 읽는 경우
트랜잭션 A가 롤백했을 시, 트랜잭션 B는 롤백전 변경값을 읽게 되어 데이터 불일치가 발생한다. 

#### Non-Repeatable Read
같은 쿼리를 두 번 수행할 예정인 트랜잭션이, 다른 트랜잭션에 의해서 첫번째 쿼리 결과와 두번째 쿼리 결과가 달라 발생하는 데이터 불일치

#### Phantom Read
일정 범위 내의 데이터를 조회하는 쿼리를 두 번 수행하는 트랜잭션이 다른 트랜잭션에 의해 다른 데이터를 조회하면서 발생하는 데이터 불일치

<br>

=> 동시성 문제를 해결하기 위한 Isolation 변경 

<br>

## Isolation 
격리수준 : 트랜잭션에서 일관성이 없는 데이터를 허용하는 수준

<br>

- 기본설정 : DB의 Isolation Level에 따라 결정
- READ_UNCOMMITTED (Level 0) : 공유 LOCK 없음, 트랜잭션이 커밋되지 않은 데이터를 다른 트랜잭션이 READ할 수 있음
- READ_COMMITTED (Level 1) : 트랜잭션이 커밋되어 확정된 데이터만 읽도록 허용 
- REPEATABLE_READ (Level 2): 선행 트랜잭션이 읽은 데이터를 LOCK걸어둠. 해당 트랜잭션이 종료시까지 UPDATE/DELETE 불가능, INSERT 가능
- SERIALIZABLE (Level 3) : Index에 공유 LOCK 설정하여 다른 트랜잭션의 INSERT문까지 금지 

<br>

LEVEL0 : Dirty Read 발생 가능 <br>
LEVEL0, LEVEL1 : Non-Repeatable Read 발생 가능 <br>
LEVEL0, LEVEL1, LEVEL2 : Phantom Read 발생 가능 <br>

<br>

## Propagation 
전파옵션 : 트랜잭션 도중 다른 트랜잭션을 호출하는 상황에서 선택할 수 있는 옵션

<br>

- REQUIRED : 부모 트랜잭션 내에서 실행, 무모 트랜잭션이 없는 경우 새로운 트랜잭션 생성
- SUPPORTS : 이미 시작된 트랜잭션이 있는 경우 해당 트랜잭션에 참여, 없는 경우 없는 상태로 진행
- REQUIRES_NEW : 부모 트랜잭션의 여부와 관계없이 새로운 트랜잭션 생성
- MANDATORY 
- REQUIRES_NEW
- NOT_SUPPORTED
- NEVER
- NESTED

<br>

## Read-Only 
읽기 전용 트랜잭션 설정 
읽기 이외의 동작(INSERT, UPDATE, DELETE)이 발생하면 예외 발생

<br>

## 트랜잭션 롤백 예외

기본적으로 런타임 예외(Unchecked) 발생 시 롤백하고, Checked Exception 발생 시에는 커밋한다. <br>
왜 Checked Exception 는 롤백시키지 않을까 ? >> https://cheese10yun.github.io/checked-exception/#checked-exception-rollback <br>
특정 예외가 발생했을 때 롤백을 하거나, 롤백을 하지 않도록 설정할 수 있다.

<br>

- Rollback-for : 설정한 예외가 발생 시, 롤백 수행 O (체크 예외도 설정 가능) 
- NoRollback-for : 설정한 예외가 발생 시, 롤백 수행 X


---
**참고**  <br>
https://goddaehee.tistory.com/167 <br>
http://blog.skby.net/dirty-read/ about dirty read
