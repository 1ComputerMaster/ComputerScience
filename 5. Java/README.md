# JAVA


## 1. JPA N+1 문제

> **정의** : JPA에서 연관된 엔티티를 조회할 때 발생하는 성능 문제입니다.
> 
> **상황** : 하나의 엔티티를 조회할 때 연관된 엔티티를 지연 로딩 (Lazy Loading) 설정으로 조회하면, 연관된 엔티티마다 추가적인 쿼리가 실행되어 성능이 저하됩니다.
> 
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

### 캡슐화(Encapsulation)

**정의** : 객체의 데이터와 메서드를 하나로 묶고, 외부에서 직접 접근 하지 못하도록 은닉하는 것

**장점** 

- 데이터 무결성 유지 가능
- 내부 구현을 감추어 변경에 유연함


### 상속 (Inheritance)

**정의** : 기존 클래스의 속성과 특징을 재사용하는 것

**장점**

- 코드 재사용성을 높입니다.
- 계층 구조를 통해서 시스템을 구조화 합니다.

### 다형성 (Polymorphism)

**정의** : 하나의 메서드나 클래스가 다양한 방법으로 동작하는 능력

**예시**

- **오버라이딩** : 부모 클래스의 메서드를 자식 메서드에서 재정의
- **오버 로딩** : 같은 메서드 명을 가진채로 리턴 값이 다른 메서드를 정의

### 추상화 (Abstraction)

**정의** : 필요한 특성만을 나열하고, 불필요한 세부 사항은 숨기는 것.

**방법** : 추상 클래스와 인터페이스를 통해서 나타냄

**장점** : 복잡성을 줄이고, 코드 유지 보수성을 높입니다.

## 4. SOLID 원칙

### 단일 책임 원칙 (Single Resposibility Principle)

**정의** : 클래스는 하나의 책임만 가져야 합니다.

**예시** : 사용자 인증과 이후 비즈니스 로직의 데이터 처리를 별도 클래스로 분리합니다.

**장점** : 변경에 유연하고, 유지보수가 용이합니다.

### 개방-폐쇄 원칙 (Open/Closed Principle)

**정의** : 확장에는 열려있고 수정에는 닫혀 있어야 합니다.

**예시** : 새로운 기능을 추가할 때 기존 코드를 수정하지 않고 새로운 클래스를 추가합니다.

**장점** : 시스템의 안정성을 높이고, 변경에 강합니다.

### 리스코프 치환 원칙 (Liskov Substitution Principle)

**정의** : 서브 타입은 언제나 자신의 기반 타입으로 교체할 수 있어야 합니다.

**예시** : 부모 클래스의 기능을 해치지 않도록 자식 클래스를 구현합니다.

**장점** : 상속 구조에서 일관성을 유지합니다.

### 인터페이스 분리 원칙 (Interface Segregation Principle)

**정의** : 클라이언트는 자신이 사용하지 않는 메서드에 의존하지 않아야 합니다.

**예시** : 하나의 거대한 인터페이스를 만들어서 사용하지 않는 메서드 의존을 사용하면 안됩니다. 하나하나 추상화 단위를 낮춰서 사용하지 않는 메서드 의존을 막습니다.

**정리** : 시스템의 결합도를 낮추고 유연성을 높입니다.

### 의존 역전 원칙 (Dependency Inversion Principle)

**정의** : 고수준 모듈은 저수준 모듈에 의존해서는 안되며, 둘 다 추상화에 의존해야 합니다.

**예시** : 구현 클래스가 아닌 인터페이스나 추상 클래스에 의존하도록 설계합니다.

**장점** : 변경에 유연하고, 모듈 간의 결합도를 낮춥니다.

DIP를 고려하지 않은 코드

```java

// 1. 저수준 모듈
public class EmailService {
    public void sendEmail(String message) {
        System.out.println("Sending email: " + message);
    }
}

// 2. 고수준 모듈
public class NotificationService {
    private EmailService emailService;

    public NotificationService(EmailService emailService) {
        this.emailService = emailService;
    }

    public void send(String message) {
        emailService.sendEmail(message);
    }
}

```
DIP를 고려해서 쓴 코드
```java
// 1. 추상화된 인터페이스 (고수준 모듈과 저수준 모듈 간의 추상화 계층)
public interface MessageService {
    void sendMessage(String message);
}

// 2. 저수준 모듈 - EmailService
public class EmailService implements MessageService {
    @Override
    public void sendMessage(String message) {
        System.out.println("Sending email: " + message);
    }
}

// 3. 저수준 모듈 - SMSService
public class SMSService implements MessageService {
    @Override
    public void sendMessage(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

// 4. 고수준 모듈
public class NotificationService {
    private MessageService messageService;

    public NotificationService(MessageService messageService) {
        this.messageService = messageService;
    }

    public void send(String message) {
        messageService.sendMessage(message);
    }
}

```


## 5. Design Pattern

### 싱글톤 패턴 (Singleton Pattern)

#### 목적
- 클래스의 인스턴스를 하나만 생성하여 전역적으로 접근 할 수 있게 합니다.

#### 구현 방법
- 생성자를 private로 선언하여 외부에서 인스턴스를 생성하지 못하게 합니다.
- 클래스 내부에 유일한 인스턴스를 생성하고 이를 생성하고 반환하는 정적 메서드를 제공합니다.
- @Component 를 객체에 붙여서 비즈니스 코어쪽에서 사용 할 때 생성자 호출 패턴 등으로 사용합니다.

#### 주의사항
- 멀티스레드 환경에서 동기화 처리가 필요합니다.

### 팩토리 패턴 (Factory Pattern)

#### 목적
- 객체 생성 코드를 캡슐화하여 코드의 의존성을 낮추고 유연성을 높입니다.

#### 구현 방법
- 객체 생성을 담당하는 팩토리 클래스를 만들고, 클라이언트는 팩토리 클래스의 메서드를 불러와서 객체를 생성함

#### 사용 예시
- 다양한 하위 클래스의 객체를 생성해야하지만, 구체적인 클래스에 의존하고 싶지 않을 때 사용합니다.
- 저는 개인적으로 Hexagonal Architecture 적용 시 같은 인터페이스를 구현한 객체에 대해서 Factory Pattern으로 리턴하였습니다.
- 서비스 로직에서는 type 정도만 정의해서 넘겨주면 Redis를 사용해서 save 할 지 MongoDB로 save 할 지 등을 구현 했습니다.

### 옵저버 패턴 (Observer Pattern)

#### 목적
- 한 객체의 상태 변화가 있을 때, 의존성 잇는 다른 객체에 자동으로 알리고 갱신할 수 있게 하는 패턴

#### 구현 방법

- Subscribe는 Observer 객체의 변화 값을 가지고 있습니다.
- Publisher는 Observer 객체를 생성하고 이를 등록하고 사용 및 업데이트 합니다.

#### 사용 예시

- 이벤트 리스너, GUI의 이벤트 처리, 실시간 데이터 업데이트

## 6. JVM 구조와 Garbage Collection 매커니즘

### JVM 구조

#### 클래스 로더 시스템
- **클래스 로더**가 클래스 파일을 읽어 메소드 영역에 로드 합니다.
- 로딩, 링크, 초기화 단계를 거쳐 클래스가 준비됩니다.

#### 실행 엔진
- **인터 프리터** : 바이트코드를 한 줄씩 해석하여 실행합니다.
- **JIT 컴파일러** : 자주 실행되는 코드 블록을 기계어로 컴파일하여 성능을 향상 시킵니다.

#### 메모리 영역 : HEAP / STACK / 메소드 영역 / PC 레지스터 / 네이티브 메서드 스택 

- **메소드 영역** : 클래스가 로딩이 될 때 사용되며 주로 static 변수 , 클래스의 메타데이터 (클래스 명 / 부모 클래스 명 / 메서드 정보 / 필드 정보) 그리고, 상수가 저장됩니다. 
- **힙 영역** : 애플리케이션 내부에서 생성되는 모든 객체(기본형 제외)와 배열이 저장되는 영역
- **스택 영역** : 각 스레드 마다 생성이 되며, 메서드 호출 시에 사용되는 프레임 (따라서, 지역 변수 및 매개 변수가 저장됩니다.)
- **PC 레지스터** : 각 스레드마다 생성되며, 현재 실행 중인 JVM 명령어의 주소가 저장됨
- **네이티브 메서드 스택** : JAVA Code가 아닌 (C,C++, etc...) 비자바 코드가 실행 될 때 활용하는 스택 영역


### Garbage Collection

#### 역할
- 사용되지 않는 객체를 메모리에서 자동으로 해제하여 메모리 누수를 방지

#### 동작 원리
**Mark and Sweep Algorithm**
- **Mark 단계** : 모든 객체를 탐색하여 사용 중인 객체에 마크합니다.
- **Sweep 단계** : 마크되지 않은 모든 객체를 메모리에서 제거합니다.

#### Generation 방식

- 힙 영역을 Young Generation과 Old Generation으로 나누어 관리합니다.
- **Young Generation** : 새로운 객체가 할당 되는 영역 (Minor GC 발생)
  - **Eden Space** : 처음 할당 된 객체가 위치하는 곳으로 가장 시기가 짧고 이때 GC를 통해서 빠르게 처리 할 수 있는 객체가 많아야 추후에 Major GC를 최소화 할 수 있다.
  - **Survivor Space(S0,S1)** : Eden Space에서도 생존한 객체들이 여기에 해당 되며 S0 -> S1까지 버티면 Old Generation으로 승격되어 관리된다. 
- **Old Generation** : 오래된 객체가 저장되는 영역 (Major GC 발생)
  - 해당 영역이 GC가 자주 일어나면 일시 중단이 발생 할 수 있고 순단이 일어 날 수 있으니 주의가 필요하다.

#### Garbage Collection 종류

- **Serial GC** : 단일 스레드로 GC를 수행
- **Parallel GC** : 멀티스레드로 GC 수행
- **CMS GC** : 애플리케이션 스레드와 병행하여 GC 수행하여 일시 중단 시간을 줄임
- **G1 GC** : 힙을 여러 영역으로 나누어 병렬적으로 GC를 수행 (Java ver 9 이상 일 시 Default)