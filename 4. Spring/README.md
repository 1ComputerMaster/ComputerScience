# Spring

### 1. DI (Dependency Injection, 의존성 주입)

> **정의** : Spring Framwork에서 볼 수 있는 특이한 패턴으로 객체 간의 의존 관계를 직접 설정하는 것이 아니라, 외부에서 필요한 객체를 주입하여 사용하도록 하는 디자인 패턴입니다.

#### 장점
- 코드 유지 보수가 용이하고 가독성이 높아집니다.
- 코드 결합도를 낮춰 유연성 및 테스트가 용이 해집니다.

#### 예시
- Spring Framework에서 생성자에 @Autowired 선언 후 final로 선언된 의존 객체에 대해서 의존성 주입을 시킵니다. 또는, Lombok의 @RequiredArgsConstructor을 씁니다. 보통 생성자 주입이 권장됩니다. 그 이유는 필드 주입 시 외부에서 해당 객체 접근이 불가능합니다. 보통 테스트 케이스 쓰기가 어렵습니다. 그리고, 서로가 서로의 객체를 바라보는 순환 참조 문제가 생길 수 있습니다.

### 2. AOP (Aspect-Oriented Programming, 관점 지향 프로그래밍)

> **정의** : 비즈니스 로직에서 공통적인 기능 (로깅, 보안, 트랜잭션 관리 등)을 분리하여 모듈화하고 재사용성을 높이기 위한 프로그래밍 패러다임 입니다.

#### 작동 방식
- **Aspect** : 공통 기능을 정의한 모듈
- **Advice** : 실제로 적용할 공통 기능 (로직)
- **Join Point** : Advice가 실행될 지점
- **Pointcut** : Advice를 적용할 조건

#### 장점 : 
중복된 코드를 줄일 수 있고, 비즈니스 로직과 공통 기능을 분리해 코드의 가독성과 유지보수를 향상 시킵니다.

### 3. Application Context (어플리케이션 컨텍스트)

> **정의** : Spring에서 빈(Bean) 객체를 관리하고 제공하는 컨테이너
#### 역할 
- 빈의 생성, 관리, 의존성 주입을 담당함
- 이벤트 전달, 메시지 리소스 핸들링, 환경 정보 등을 관리합니다.


### 4. IoC 컨테이너란?
> **정의** : **IoC (Inversion of Control)**는 객체의 생성과 의존성 관리를 개발자가 직접 수행하는 대신, 프레임워크가 이를 맡아 처리한다는 개념입니다. Spring 프레임워크에서는 IoC 컨테이너가 이 역할을 수행합니다. 
> 
>  IoC 컨테이너는 애플리케이션에 필요한 객체(빈, Bean)를 생성하고, 이들 간의 의존성 주입(Dependency Injection)을 관리하는 핵심 구성 요소입니다. 이를 통해 개발자는 객체 생성과 의존성 관리에 대한 고민 없이 비즈니스 로직에 집중할 수 있습니다.

**IoC 컨테이너의 역할**

- **객체 생성**: 개발자가 직접 객체를 생성하는 것이 아니라, 컨테이너에 객체 생성 방법을 등록해 두면, 필요할 때 IoC 컨테이너가 해당 객체를 생성합니다.

- **의존성 관리**: 객체들 간의 의존 관계를 IoC 컨테이너가 설정합니다. 이를 통해 객체들은 서로 직접적인 의존성을 갖지 않고, 컨테이너를 통해 필요한 의존성을 주입받게 됩니다.

- **객체 수명 주기 관리**: 객체의 생성, 소멸 등 전체 생명 주기를 컨테이너가 관리합니다.

### 5. 빈(Bean)이란?
> **정의** : **빈(Bean)**은 IoC 컨테이너에 의해 생성되고 관리되는 객체를 의미합니다. Spring에서는 빈 객체를 XML, 자바 어노테이션(@Component, @Service, @Repository, @Controller) 또는 자바 설정 파일(@Configuration) 등을 통해 정의하고 관리합니다.
> 
> 빈은 싱글톤(Singleton) 패턴을 기본으로 사용하므로, 애플리케이션 실행 동안 빈을 하나만 생성하여 여러 곳에서 재사용할 수 있습니다. 이로 인해 메모리 효율성을 높이고 일관된 객체 상태를 유지할 수 있습니다.

**빈 생성과 의존성 주입 관리란?**
IoC 컨테이너를 사용해 빈을 생성하고 의존성을 주입하는 과정을 살펴보겠습니다:

#### 1. 빈 생성
Spring IoC 컨테이너는 개발자가 정의한 설정(@Component, @Service, @Repository, @Bean 등)에 따라 빈을 생성합니다. 예를 들어, 아래의 코드를 통해 Car 클래스가 빈으로 등록될 수 있습니다:

```java
@Component
public class Car {
    public void start() {
        System.out.println("Car is starting.");
    }
}
```

- 위 코드에서 @Component 어노테이션을 사용하면, Spring IoC 컨테이너는 Car 객체를 빈으로 자동 생성하여 관리합니다.

#### 2. 의존성 주입 (Dependency Injection)

- 의존성 주입이란 객체가 필요한 의존성을 IoC 컨테이너로부터 주입받는 것을 의미합니다. Spring에서는 @Autowired 어노테이션을 통해 의존성 주입을 간편하게 처리할 수 있습니다.

``` java

@Component
public class Garage {

    private final Car car;

    // 생성자 주입
    @Autowired
    public Garage(Car car) {
        this.car = car;
    }

    public void openGarage() {
        car.start();
        System.out.println("Garage is open.");
    }
}
```
- 위 코드에서 Garage 클래스는 Car 클래스에 의존하고 있습니다.

- @Autowired 어노테이션을 사용하면 Spring IoC 컨테이너가 Car 빈을 생성자에 주입해줍니다.

- 이렇게 하면 개발자는 직접 Car 객체를 생성하거나 관리할 필요가 없고, 대신 IoC 컨테이너가 Car 객체를 생성하여 Garage에 주입합니다.

#### 정리
- IoC 컨테이너는 객체의 생성 및 의존성을 관리하여 애플리케이션의 결합도를 낮춥니다.

- **빈(Bean)**은 IoC 컨테이너에 의해 생성 및 관리되는 객체이며, 재사용성을 높이고 상태 일관성을 유지합니다.

- 의존성 주입은 IoC 컨테이너가 객체 간의 의존성을 자동으로 연결해주는 것을 의미하며, 이는 개발자가 객체를 직접 생성하고 관리하는 부담을 덜어줍니다.

- 이를 통해 개발자는 객체 생성 및 의존성 관리에 신경 쓸 필요 없이 비즈니스 로직 구현에 집중할 수 있으며, 애플리케이션의 유연성과 유지보수성이 향상됩니다.


### 6. API Gateway란?

- API Gateway는 MSA에서 클라이언트 요청을 적절한 서비스로 라우팅하고 공통 기능을 수행하기 위해서 만들어진 서비스

#### 역할

- **요청 라우팅** : 클라이언트 요청을 각 마이크로서비스로 전달

- **인증 및 권한 부여** : JWT Token 인증 시스템을 API Gateway에 구성하여 이를 공통으로 빼서 쓸 수 있습니다. JWT Token 검증 가능 또는, **Spring Security**의 권한에 따라서 요청을 필터링 합니다. Admin이면 Admin Page 접근등

- **요청 제한 및 모니터링** : 단위 시간당 요청 수를 제한하여 서비스 남용을 방지합니다. 트래픽 추적 및 에러 로그 수집 등으로 시스템 상태를 파악합니다.
  - 요청 제한 : 애초에 받을 때 부터 받을 수 있는 최대 Client 수를 지정해서 받는 것임 CircuitBreaker와는 다른 개념으로 서킷은 백엔드 서비스에서 다른 MSA 내부 서비스 장애 포인트로 연결되지 않게끔 Timeout 장애를 연쇄적으로 일으키는 것을 막는 것이고 Rate Limit와는 다름

- **캐싱 및 로드 밸런싱** : 자주 요청되는 데이터를 캐싱하여 성능을 향상 시키고 백엔드 서비스로 트래픽을 균등히 배분합니다. (Round Robin)
 - 여기서 의미하는 응답 캐싱은 AWS API GATEWAY 기능으로 사용 가능함
 - 스프링 Cloud에서 구현 하려면 API Gateway 구현 시 REDIS 연동 후 TTL 걸어서 가져오는 식으로 해야 할 것임

### 7. Service Discovery

#### 정의

- 서비스 디스커버리는 동적으로 변화하는 마이크로 서비스의 네트워크 위치(IP와 포트)를 찾을 수 있게 해주는 매커니즘
 

#### 원리

1. 서비스 레지스트리 : **서비스 레지스트리**는 각 서비스의 인스턴스의 네트워크 위치 정보를 기억하는 데이터베이스
 - Eureka 서버에서는 Inmemory 형태로 데이터 베이스를 기억하고 이를, JVM에 Heap 메모리에서 기억합니다. (기본형 객체가 아니니 당연히 Heap에 저장됨)

2. 서비스 등록 : 서비스 인스턴스 (User Service, Product Service, ...)가 시작 될 때, 서비스 레지스트리에 등록되고 헬스체크를 통해서 서비스 가용성을 지속적으로 확인 (Actuator)

3. 서비스 조회 : 클라이언트 또는 API Gateway가 서비스 레지스트리에서 목적 서비스의 위치를 조회함

#### 구현 방식

1. 클라이언트 사이드 디스커버리

- **클라이언트**가 서비스 레지스트리에서 직접 서비스 위치를 조회하고 요청을 전달.
- 로드 밸런싱을 클라이언트 측에서 연동 (Application 주도)

2. 서버 사이드 디스커버리

- 클라이언트 요청이 **로드 밸런서**로 전달이 되고 로드 밸런서가 서비스 레지스트리에서 위치를 조회해서 요청을 전달함 (Kubernetes -> Service 등록 시 해당 서비스의 포트 및 service name등으로 판단 가능)


### 8. **JWT (JSON Web Token)란?**
JWT는 JSON 포맷을 사용하여 정보를 안전하게 주고받기 위한 웹 표준 (RFC 7519)입니다. 주로 **인증** 및 **인가**(Authorization) 목적으로 많이 사용됩니다. JWT는 서명된 토큰이기 때문에 변조가 어려우며, 서버는 토큰의 서명을 통해 신뢰할 수 있는 정보를 전달받을 수 있습니다.

#### 1. **JWT의 구성**
JWT는 크게 세 부분으로 나뉩니다:
- **Header**: 토큰의 타입 (JWT) 및 해싱 알고리즘 정보를 담고 있습니다. (예: HS256, RS256 등)
- **Payload**: 토큰에 담길 실제 데이터로 **Claim**이라고 부릅니다. 이곳에 인증된 사용자에 대한 정보(예: 사용자 ID, 권한 등)를 저장할 수 있습니다. 하지만, 암호화되지 않기 때문에 중요한 정보는 담지 않습니다.
- **Signature**: 토큰이 변조되지 않았음을 보장하기 위한 서명 부분입니다. 서버의 **비밀키**를 사용하여 헤더와 페이로드를 서명한 값입니다. 

#### 예시

```plaintext
header.payload.signature
```


```plaintext
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

#### 2. **JWT의 작동 방식**

1. **사용자 로그인 요청**: 사용자가 로그인하면 서버는 사용자의 인증 정보를 확인하고, 성공 시 사용자 정보를 담은 JWT를 발급합니다.
2. **토큰 전달**: 발급된 JWT는 클라이언트(주로 브라우저 또는 모바일 앱)에 저장됩니다. 이때, 클라이언트는 일반적으로 JWT를 **쿠키**나 **로컬 스토리지**에 저장합니다.
3. **인증된 요청**: 이후 클라이언트는 서버에 요청할 때마다 JWT를 포함하여 보냅니다. (예: HTTP 헤더의 Authorization 필드에 Bearer Token으로 전달)
   ```http
   Authorization: Bearer <JWT 토큰>
   ```
4. **서버에서 검증**: 서버는 요청을 받을 때 JWT의 **서명(Signature)**을 검증하여 토큰이 변조되지 않았는지 확인합니다. 서명이 유효하면 토큰에 담긴 정보를 사용해 사용자를 인증합니다.
5. **인가 (Authorization)**: 서버는 JWT의 Payload에 담긴 사용자 권한 정보를 바탕으로 클라이언트의 권한을 확인한 후, 요청된 리소스에 접근할 수 있는지 여부를 결정합니다.

#### 3. **JWT의 장점**
1. **Stateless**:
   - 서버가 세션을 유지할 필요가 없습니다. 즉, 서버에서 클라이언트의 인증 상태를 저장하지 않으므로 서버 자원을 절약할 수 있습니다. 이 때문에 확장성이 높은 애플리케이션에서 적합합니다.
2. **확장성**:
   - 마이크로서비스 아키텍처에서 각 서비스 간 인증을 위해 유용하게 사용될 수 있습니다. 한 번 발급된 JWT는 여러 서비스에 공통으로 사용할 수 있습니다.
3. **자체 포함(self-contained)**:
   - JWT는 필요한 모든 정보를 토큰에 담고 있으므로 별도의 데이터베이스 조회 없이 사용자에 대한 정보를 확인할 수 있습니다.
   
#### 4. **JWT의 단점**
1. **탈취 시 위험**:
   - 클라이언트 측에 JWT가 저장되기 때문에 토큰이 유출되면 탈취한 사람이 그대로 사용할 수 있습니다. 이를 방지하기 위해 HTTPS 사용 및 만료 기간 설정이 중요합니다.
2. **길이 문제**:
   - JWT는 자체적으로 데이터를 담고 있기 때문에, 페이로드에 많은 정보를 담을수록 토큰의 크기가 커집니다. 토큰이 커지면 네트워크 트래픽이 증가할 수 있습니다.
3. **만료 후 처리**:
   - JWT는 세션 기반 인증처럼 서버에서 강제로 만료시킬 수 없습니다. 이를 해결하기 위해서는 **만료 시간(expiration time, `exp`)**을 적절히 설정하고, 필요 시 **블랙리스트**를 구현해야 합니다.

#### 5. **JWT의 활용 예시**
1. **사용자 인증**: 
   - 사용자가 로그인할 때 발급된 JWT를 요청에 포함시켜 서버에서 인증 및 인가를 처리합니다.
2. **API 인증**:
   - API 서버가 여러 클라이언트와 통신할 때, JWT를 사용해 클라이언트를 인증하고, 권한을 부여할 수 있습니다.
3. **OAuth2와 함께 사용**:
   - OAuth2 인증 플로우에서 액세스 토큰으로 JWT를 사용하는 경우가 많습니다. 이 방식은 마이크로서비스 환경에서 잘 맞으며, 각 서비스 간에 JWT를 통해 인증 상태를 공유할 수 있습니다.

#### 6. **JWT 보안 고려 사항**
1. **HTTPS**:
   - JWT는 민감한 정보를 포함할 수 있으므로 HTTPS를 사용해 통신을 암호화하는 것이 필수적입니다.
2. **짧은 만료 시간 설정**:
   - JWT는 영구적으로 유효하지 않도록 적절한 **만료 시간**을 설정해야 합니다. 짧은 만료 시간을 설정함으로써, 토큰 탈취 시 피해를 줄일 수 있습니다.
3. **토큰 무효화**:
   - 서버 측에서 특정 JWT를 블랙리스트에 등록하여 해당 토큰의 사용을 금지하는 방법을 구현할 수 있습니다.
4. **서명 알고리즘 선택**:
   - HMAC SHA-256(HS256)과 같은 대칭키 알고리즘뿐만 아니라 RSA 또는 ECDSA와 같은 비대칭키 알고리즘을 사용하여 보안성을 강화할 수 있습니다.



### 9. **함수형 인터페이스 (Functional Interface)**

> **정의**: 함수형 인터페이스는 하나의 **추상 메서드**만을 가지는 인터페이스로, Java 8부터 도입된 **람다 표현식**을 활용할 수 있는 기반입니다. <br/>
> 자바에서는 @FunctionalInterface 어노테이션을 사용하여 함수형 인터페이스임을 명시할 수 있습니다.

---

### **1. 특징**

1.  **단일 추상 메서드**: 함수형 인터페이스는 하나의 추상 메서드만을 선언할 수 있습니다.
2.  **람다 표현식** 지원\*\*: 함수형 인터페이스는 Java 8의 람다 표현식과 함께 사용되며, 코드 가독성과 간결성을 향상시킵니다.
3.  **Object 클래스의 메서드 포함 가능**: toString(), equals() 등 Object 클래스에서 상속받은 메서드는 추상 메서드 개수에 영향을 미치지 않습니다.

---

### **2. @FunctionalInterface 어노테이션**

-   Java는 @FunctionalInterface 어노테이션을 제공하여 해당 인터페이스가 함수형 인터페이스임을 명시할 수 있습니다.
-   컴파일러가 단일 추상 메서드를 가지는지 확인하여 함수형 인터페이스로서의 규칙을 강제합니다.
-   선택사항이지만, 사용을 권장합니다.

---

### **3. 함수형 인터페이스 예시**

#### 1. 사용자 정의 함수형 인터페이스

java
```
@FunctionalInterface 
public interface Calculator 
{ 
   int calculate(int a, int b); 
}
```
-   Calculator는 두 개의 정수를 입력받아 계산 결과를 반환하는 함수형 인터페이스입니다.

#### 2. 람다 표현식으로 사용

java
```
public class Main {
    public static void main(String []args) {
        Calculator addition = (a, b) -> a + b;
        Calculator multiplication = (a, b) -> a * b;
        System.out.println("Addition: " + addition.calculate(5, 3));  //8
        System.out.println("Multiplication: " + multiplication.calculate(5, 3)); //15 
   }
}
```
---

### **4. Java에서 제공하는 함수형 인터페이스**

Java는 java.util.function 패키지에 다양한 표준 함수형 인터페이스를 제공합니다.

인터페이스추상 메서드설명

| Interface | Method | Description |
| --- | --- | --- |
| Predicate | boolean test(T t) | 입력값을 평가하여 true 또는 false를 반환합니다. |
| Function<T, R> | R apply(T t) | 입력값을 특정 연산을 수행하여 결과로 변환합니다. |
| Consumer | void accept(T t) | 입력값을 소비하며 반환값이 없습니다. |
| Supplier | T get() | 매개변수 없이 값을 반환합니다. |
| BiFunction<T, U, R> | R apply(T t, U u) | 두 개의 입력값을 받아 결과로 변환합니다. |
| UnaryOperator | T apply(T t) | 입력값을 동일한 타입으로 반환하는 연산을 수행합니다. |
| BinaryOperator | T apply(T t1, T t2) | 동일한 타입의 두 값을 받아 연산을 수행합니다. |

#### 예시: Predicate 활용

java
```
import java.util.function.Predicate; 
public class PredicateExample { 
   public static void main(String [] args) 
   { 
      Predicate isEmpty = String::isEmpty; System.out.println(isEmpty.test("")); // true 
      System.out.println(isEmpty.test("Hello")); // false 
   } 
}
```
---

### **5. 함수형 인터페이스 활용**

#### 1. 스트림 API와 함께 사용

함수형 인터페이스는 Java의 스트림 API에서 핵심적으로 사용됩니다.

java
```
import java.util.Arrays; 
import java.util.List; 
import java.util.stream.Collectors; 
public class StreamExample { 
   public static void main(String [] args) { 
      List numbers = Arrays.asList(1, 2, 3, 4, 5); 
      List evenNumbers = numbers.stream() .filter(n -> n % 2 == 0) // Predicate 사용 
      .collect(Collectors.toList()); 
      System.out.println(evenNumbers); //[2, 4] 
   } 
}
```
#### 2. 메서드 참조와 결합

람다 표현식 대신 메서드 참조를 활용하여 가독성을 높일 수 있습니다.

java
```
import java.util.function.Consumer; 
public class MethodReferenceExample { 
   public static void main(String [] args) { 
      Consumer printer = System.out::println; 
      printer.accept("Hello, World!"); // "Hello, World!" 
   } 
}
```
---

### **6. 함수형 인터페이스의 장점**

1.  **코드 간결성**: 불필요한 익명 클래스를 제거하여 코드가 더 간결하고 읽기 쉬워집니다.
2.  **가독성 향상**: 람다 표현식 및 메서드 참조로 의도가 명확하게 전달됩니다.
3.  **재사용성**: 공통 동작을 추상화하여 재사용 가능성을 높입니다.
4.  **스트림 API 지원**: 컬렉션 처리와 같은 Java 8 기능에서 필수적인 역할을 수행합니다.

---

### **7. 함수형 인터페이스 활용 시 유의점**

1.  **단일 추상 메서드만 포함**:
    -   함수형 인터페이스에는 하나의 추상 메서드만 선언해야 합니다. 컴파일러는 추가 메서드 선언 시 오류를 발생시킵니다.
2.  **@FunctionalInterface 사용 권장**:
    -   의도하지 않게 다중 추상 메서드를 추가하는 실수를 방지할 수 있습니다.
3.  **무상태 설계 권장**:
    -   함수형 인터페이스는 상태를 가지지 않도록 설계하는 것이 좋습니다. 상태가 있을 경우 동시성 문제를 일으킬 수 있습니다.

---

### **8. 면접에서 예상 질문**

1.  **함수형 인터페이스란 무엇인가요?**
    -   함수형 인터페이스는 하나의 추상 메서드를 가지는 인터페이스로, 람다 표현식과 함께 사용됩니다. Java 8에서 도입되었으며, @FunctionalInterface 어노테이션을 통해 선언됩니다.
2.  **익명 클래스와 함수형 인터페이스의 차이점은 무엇인가요?**
    -   함수형 인터페이스는 람다 표현식과 함께 사용하여 코드가 간결하며, 의도를 명확히 전달할 수 있습니다. 반면 익명 클래스는 더 많은 코드와 boilerplate를 필요로 합니다.
3.  **람다 표현식이 함수형 인터페이스에 적합한 이유는 무엇인가요?**
    -   람다 표현식은 함수형 인터페이스의 단일 추상 메서드를 구현하기 위한 간단하고 직관적인 구문을 제공합니다.
4.  **Predicate와 Function의 차이는 무엇인가요?**
    -   Predicate는 boolean 값을 반환하는 함수형 인터페이스이고, Function은 입력값을 처리하여 결과값으로 변환합니다.

