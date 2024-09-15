# JAVA


## 1. JPA N+1 문제

> **정의** : JPA에서 연관된 엔티티를 조회할 때 발생하는 성능 문제입니다.
> **상황** : 하나의 엔티티를 조회할 때 연관된 엔티티를 지연 로딩 (Lazy Loading) 설정으로 조회하면, 연관된 엔티티마다 추가적인 쿼리가 실행되어 성능이 저하됩니다.
> **예시** : 회원 리스트를 조회하면서 각 회원이 가진 주문 목록을 조회하는 경우, 회원 수가 N개라면 회원을 가져오는 1개의 쿼리와 각 회원의 주문 목록을 가져오는 N개의 쿼리로 총 N + 1개의 쿼리가 생성됩니다.

### 해결 방법
- 패치 조인 (Fetch Join) : 연관된 엔티티를 한 번의 쿼리로 가져오는 방법
- 패치 조인 상세 : JOIN FETCH 문을 이용해서 이를 최적화 할 수 있습니다.
    ```
    String jpql = "SELECT m FROM Member m JOIN FETCH m.orders";
    List<Member> members = entityManager.createQuery(jpql, Member.class).getResultList(); //이렇게 설정하면 아래와 같이 쿼리를 만듭니다.

    SELECT m.*, o.*
    FROM member m
    LEFT JOIN order o ON m.id = o.member_id

    ```
- 엔티티 그래프 (Entity Graph) : 연관된 엔티티를 가져올 때 필요한 속성을 명시하는 방법 


## 2. ORM (Object-Relational Mapping) : JPA, Hibernate의 동작 원리와 주의 사항

### ORM 이란?

- 객체 지향 프로그래밍 언어의 객체와 데이터베이스 테이블간의 **매핑**을 자동으로 해주는 기술

### JPA와 Hibernate

- JPA(Java Persistence API) : 자바에서 ORM을 위한 표준 인터페이스를 정의한 사양
- Hibernate : JPA의 구현체 중 하나로, 가장 널리 사용됨

### 동작 원리

#### 1. 엔티티 매핑
- 자바 클래스(엔티티)를 DB 테이블과 메핑
- 클래스 필드 == column

#### 2. 영속성 콘텍스트
- **영속성 콘텍스트**는 데이터 베이스에서 가져온 데이터를 1차로 인 메모리에서 캐시합니다. 이를 통해서 성능 최적화가 가능합니다.

**엔티티 생명 주기 관리** 

- **비영속** (Transient) 영속성 콘텍스트에 올라가지 않아서 데이터베이스에 연동되지 않았습니다. (보통 JAVA 내부에서만 쓰이는데 Entity로 관리가 되어야 하면 Transient로 어노테이션을 걸고 이를 활용했습니다.) 
- **영속** : 영속성 컨텍스트에 반영이 되기 때문에 바로 반영됩니다.

**변경 감지(Dirty Check)**

- 영속 상태의 엔티티를 관리하기 때문에, 트랜잭션이 끝날 때 자동으로 변경된 내용을 감지하여 데이터 베이스에 반영합니다.
EntityManager.persist(); 하면 끝

**쓰기 지연(Write-Behind)** : 영속성 컨텍스트는 엔티티의 변경을 데이터베이스에 즉각 반영하는 것이 아닌 트랜잭션이 커밋되는 그 시점에 모아둔 변경 점을 일괄적으로 반영

**이점** : 동일성 보장, 성능 최적화

#### 3. 엔티티 매니저

- EntityManager는 Thread-Safe하지 않기 때문에 쓰지 않아야 한다고 하지만 @Transaction 어노테이션을 붙인 메소드에서 생성해서 쓰면 된다. 여기 내부에서는 Isolation을 보장하기 때문에 Thread-safe하며 서로 다른 스레드끼리 다른 EntityManager를 쓰게 된다. 
 - 여기서 같은 행을 수정하려는 경우에는 데이터 베이스의 동시성 제어와 격리 수준에 따라서 충돌이 처리되는데 일반적으로 데이터 베이스에서 락이 걸려서 이는 방지가 된다.
- 따라서, EntityManager에서의 관리만 되면 됩니다.

**지연 로딩(Lazy-Loading)** : 실제로 필요한 시점에 불러와서 데이터를 조회하기 때문에 효율적이다.

## 3. 객체지향 프로그래밍의 원칙 (OOP) 원칙
