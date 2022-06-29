# 트랜잭션(Transaction)

## 개요

---

트랜잭션은 데이터베이스 상태를 변화시키기 위한 **작업 수행의 논리적 단위**를 의미한다.

쉬운 예로 송금 작업을 들 수 있다. 송금은 크게 다음과 같은 과정으로 이뤄진다.

1. A의 잔고 확인
2. A의 잔고에서 이체금액만큼 빼주기
3. B의 잔고에 이체금액만큼 더해주기

1~3의 과정은 중간에 문제없이 한 번에 수행되어야 하는 단위이다. 예를 들어 2번까지 수행되고 3번 작업이 문제가 생겨 수행되지 않는다면, A의 계좌에는 돈이 빠져나갔지만 B계좌에는 돈이 입금되지 않은 상태가 될 것이다. 이는 심각한 장애이다.

따라서 1~3의 과정은 모두 정상적으로 수행되거나, 중간에 문제가 생기면 모두 수행되지 않아야 하는 작업이다. 즉, 작업 수행의 논리적 단위이고 이를 트랜잭션이라고 부른다.

### Commit, RollBack

---

**Commit**

![https://velog.velcdn.com/images%2Fsyh0397%2Fpost%2F3e7a2c02-b267-486e-9e49-8f69e40a2218%2F995E053D5ADE8AC410.png](https://velog.velcdn.com/images%2Fsyh0397%2Fpost%2F3e7a2c02-b267-486e-9e49-8f69e40a2218%2F995E053D5ADE8AC410.png)

- 변경된 데이터를 테이블에 영구적으로 반영하는 것.
- COMMIT을 수행하면 하나의 트랜잭션 과정을 종료하는 것이다.
- COMMIT을 수행하면 이전 데이터가 완전히 UPDATE 된다.

**RollBack**

![https://velog.velcdn.com/images%2Fsyh0397%2Fpost%2F3c8f94a2-877a-427a-b3cd-0b141314912b%2F998AAF395ADE8C141C.png](https://velog.velcdn.com/images%2Fsyh0397%2Fpost%2F3c8f94a2-877a-427a-b3cd-0b141314912b%2F998AAF395ADE8C141C.png)

- 작업 중 문제가 발생하여 트랜잭션의 처리과정에서 발생한 변경사항을 취소하는 것.
- 트랜잭션이 시작되기 이전의 상태로 되돌린다.
- 즉, 마지막 COMMIT을 완료한 시점으로 다시 돌아간다. COMMIT하여 저장한 것만 복구한다.

**SavePoint**

만약 트랜잭션 작업의 마지막에 문제가 발생하여 전체 RollBack을 수행해야 한다면, 이전의 작업들도 모두 Rollback한 뒤 다시 수행해야하기 때문에 비효율적이다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZAlvz%2Fbtrpgzeqiin%2FgbfmebyddSKi9hKGRSeJR1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZAlvz%2Fbtrpgzeqiin%2FgbfmebyddSKi9hKGRSeJR1%2Fimg.png)

트랜잭션 작업을 수행하며 성공한 지점 사이사이에 SavePoint를 두어 이를 보완할 수 있다.

작업 중 RollBack이 발생하면, `ROLLBACK TO savepoint_name`명령으로 SAVEPOINT를 지정해 놓은 곳으로 돌아갈 수 있고, 여기서부터 다시 작업을 수행할 수 있다. (SP1으로 롤백하면, 미래 시점인 SP2는 삭제된다.)

여기서 주의할 것은 ROLLBACK으로 돌아간다고 해서 SavePoint까지가 실제 DB에 저장된 것은 아니라는 점이다. 당연히 COMMIT 명령을 실행해야만 실제 디스크에 저장된다.

## 트랜잭션 상태

---

![https://velog.velcdn.com/images%2Fsyh0397%2Fpost%2F2c2fe141-5026-400a-ae3d-ecba4554f256%2FUntitled%20(34).png](<https://velog.velcdn.com/images%2Fsyh0397%2Fpost%2F2c2fe141-5026-400a-ae3d-ecba4554f256%2FUntitled%20(34).png>)

트랜잭션은 논리적으로 5가지의 상태에 있을 수 있다.

- **Active** : 트랜잭션이 현재 **실행 중인 상태**
- **Failed** : 트랜잭이 실행되다 **오류가 발생해서 중단된 상태**
- **Aborted** : 트랜잭션이 비정상 종료되어 **Rollback이 수행된 상태**
- **Partially Committed** : 트랜잭션의 연산이 마지막까지 실행되고 **Commit이 되기 직전 상태**
- **Committed** : 트랜잭션이 성공적으로 종료되어 **Commit 연산을 실행한 후**의 상태

## 트랜잭션 특징(ACID)

---

트랜잭션의 특징은 4가지로 구분된다.

- **원자성(Automicity)**
- **일관성(Consistency)**
- **격리성(Isolation)**
- **지속성(Durability)**

### **원자성(Automicity)**

위의 송금 작업처럼, 트랜잭션 내부의 작업은 모두 성공하여 commit되거나, 문제가 발생하면 모두 rollback하여 취소해야한다는 성질이 원자성이다.

<aside>
💡 **원자성 보장**
트랜젝션 내에서 데이터베이스를 수정할때 지금까지의 성공적인 상태가 롤백 이미지로 롤백 세그먼트라는 임시 영역에 저장 된다. 만약 문제가 생겨서 롤백이 일어난다면 롤백 세그먼트 내의 상태가 복구된다.

</aside>

참고로 트랜젝션 commit한 후에는 DB에 수정사항이 영구적으로 반영되기 때문에 롤백 세그먼트의 롤백 이미지도 날라간다.

### **일관성(Consistency)**

모든 트랜잭션은 일관성 있는 데이터베이스 상태를 유지해야 한다.

일관성 있는 상태(Correct State)란 도메인의 유효범위, 무결성 제약조건 등의 제약조건을 위배하지 않는 정상적인 상태를 의미한다.

예를 들어, 트랜잭션 수행 후 데이터 타입이 변경된다거나 Not Null제한인 컬럼에 Null값이 들어가있는 경우 등을 말한다.

### **격리성(Isolation)**

각각의 트랜젝션은 서로 간섭없이 독립적으로 수행되어야 한다.

하지만, 동시 요청에 대해 이 격리성을 완벽하게 보장하면 성능이 매우 악화될 수 있다. 따라서 다른 특징들과는 다르게 이 격리성은 수준을 여러 단계로 나누어서 상황에 맞게 사용할 수 있다. 이는 트랜잭션 격리수준에서 자세히 설명한다.

### 지속성**(Durability)**

트랜잭션이 성공적으로 완료되었으면, 결과는 영구적으로 반영되어야 한다.

중간에 시스템 문제가 발생해도 로그를 참고해서 성공했던 트랜잭션을 복구 할 수 있다.

## 트랜잭션 격리 수준 (Isolation Level)

---

**동시에 여러 트랜잭션**이 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 어느 수준까지 간섭할 수 있는지를 결정하는 것이다. 크게 4가지로 분류 가능하다

- **READ UNCOMMITTED :** 다른 트랜잭션에서 **커밋되지 않은 내용도 참조**할 수 있다
- **READ COMMITTED :** 다른 트랜잭션에서 **커밋된 내용만 참조**할 수 있다
- **REPEATABLE READ :** 트랜잭션에 **진입하기 이전에 커밋된 내용만 참조**할 수 있다.
- **SERIALIZABLE :** 트랜잭션에 진입하면 **락을 걸어 다른 트랜잭션이 접근하지 못하게 한다.**

아래로 갈수록 트랜잭션의 고립성과 데이터의 무결성은 높아지지만, 동시성과 성능은 저하된다. 다음은 고립성, 데이터 무결성이 깨지면서 발생하는 대표적인 문제상황 3가지다.

1. **더티 리드 (Dirty Read)**
   - 생성, 갱신, 혹은 삭제 중에 커밋 되지 않은 데이터 조회를 허용함으로써 발생하는 문제
   - 트랜잭션이 종료되면 더 이상 존재하지 않거나, 롤백되었거나, 저장 위치가 바뀌었을 수도 있는 **잘못된 데이터를 읽어들이는 현상**이다.
2. **반복 가능하지 않은 조회 (Non-Repeatable Read)**
   - 한 트랜잭션에서 같은 쿼리를 두 번 수행할 때, 두 쿼리의 **결과가 상이하게 나타나는 비일관성 현상**
   - 한 트랜잭션이 수행 중일 때 다른 트랜잭션이 값을 수정 또는 삭제함으로써 나타난다.
3. **팬텀 리드 (Phantom Read)**
   - 한 트랜잭션에서 같은 쿼리를 두 번 수행할 때, **첫 번째 쿼리에서 없던 레코드가 두 번째 쿼리에서 나타나는 현상**
   - 한 트랜잭션이 수행 중일 때 다른 트랜잭션이 새로운 레코드를 INSERT 함으로써 나타난다.

### **READ UNCOMMITTED (Level 0)**

![https://velog.velcdn.com/images%2Fguswns3371%2Fpost%2F36cffd97-cbc2-4082-8c01-709a04e2567a%2Fimage.png](https://velog.velcdn.com/images%2Fguswns3371%2Fpost%2F36cffd97-cbc2-4082-8c01-709a04e2567a%2Fimage.png)

- 각 트랜잭션의 변경 내용이 COMMIT / ROLLBACK 여부에 상관 없이 다른 트랜잭션에서 값을 읽을 수 있다.
- 정합성에 문제가 많은 격리 수준이기 때문에 사용하지 않는 것을 권장한다.
- 아래 그림과 같이 COMMIT 되지 않은 상태지만, UPDATE 된 값을 다른 트랜잭션에서 읽을 수 있다.
- 위의 3가지 문제가 모두 발생할 수 있다.

### **READ COMMITTED (Level 1)**

![https://miro.medium.com/max/1400/1*Tt9l6Lz4J-jNKyCNB1SKWg.png](https://miro.medium.com/max/1400/1*Tt9l6Lz4J-jNKyCNB1SKWg.png)

- 대부분의 RDBMS 에서 기본적으로 사용되고 있는 격리 수준이다.
- 실제 테이블 값을 가져오는 것이 아니라, Undo 영역의 백업된 레코드에서 값을 가져온다.
- Dirty Read는 발생하지 않는다.
- NON-REPEATABLE READ, PHANTOM READ가 발생할 수 있다.
  (T1 Commit이후 T2가 222를 Select하면 Busan이 아닌 Jeju를 읽게 된다)

### **REPEATABLE READ (Level 2)**

![https://miro.medium.com/max/1400/1*WHU-7ausIBpNst0k5UcazA.png](https://miro.medium.com/max/1400/1*WHU-7ausIBpNst0k5UcazA.png)

- 현재 트랜잭션 이후의 변경사항은 읽지 않는 방식이다.
  - MySQL 경우 트랜잭션마다 트랜잭션 ID 를 부여하여 해당 ID보다 작은 트랜잭션 번호에서 변경한 것만 읽게 된다.
- Undo 공간에 백업해두고 실제 레코드 값을 변경한다.
- 백업된 데이터는 불필요하다고 판단하는 시점에 주기적으로 삭제한다.
- Undo에 백업된 레코드가 많아지면, MySQL 서버의 처리 성능이 떨어질 수 있다.
- 이러한 변경 방식을 MVCC(Multi Version Concurrency Control) 라고 부른다.
- Dirty Read, Non-Repeatable Read 문제는 발생하지 않는다.
- Phantom Read는 발생할 수 있다.

### **SERIALIZABLE (Level 3)**

- 트랜잭션 간 엄격하게 Lock을 거는 방식
- 한 트랜잭션이 완료되기 전까지 다른 트랜잭션이 조회, 수정, 삭제 등의 작업이 불가하다.
- 성능이 너무 낮아 거의 사용하지 않는다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlC6dK%2Fbtq1KdymZ3h%2FqUoEjHoczQVBWeCvoSZwv1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlC6dK%2Fbtq1KdymZ3h%2FqUoEjHoczQVBWeCvoSZwv1%2Fimg.png)

## 참고

[[데이터베이스] Transaction, 트랜잭션이란?](https://wonit.tistory.com/462)

[[Database] 트랜잭션이란?](https://devjem.tistory.com/27)

[[MySQL] - 트랜잭션의 격리 수준(Isolation level)](https://zzang9ha.tistory.com/381)

[[DATABASE] 트랜잭션의 격리 수준이란?](https://medium.com/@sunnkis/database-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80%EC%9D%B4%EB%9E%80-10224b7b7c0e)
