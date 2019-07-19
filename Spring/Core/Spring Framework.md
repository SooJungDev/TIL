## 하루에 정리하는 Spring Framework Core
- 7월 11일 NHN 에서 했던 하루에 정리하는 Spring Framework Core 강의듣고 정리


### 프레임워크
- 일반적인(generic) 기능을 제공하는 추상화 계층
- 더큰 소프트웨어 개발 플랫폼에 일부로서 특정 기능을 제공
- 보편적(universal)이고, 재사용가능(reusable) 한 소프트웨어 환경
- 선택적으로 사용자가 작성한 코드로 변경이 가능
- 예를들 Spring MVC,Spring Batch, Spring Data Framework
- [Software framework](https://en.wikipedia.org/wiki/Software_framework)


### Framework vs Library 01
- 의존성 역전의 원리 IoC (inversion of control)
    - 디자인원리(Principle)
    - 낮은 결합도(Loose coupling)
    - 라이브러리와 프레임워크 차이
- 기본동작
- 확장성
    - 프레임워크가 열어둔 기능에 대해서 사용자는 오버라이드 할 수 있다
    
### Framework vs Library 02
- Framework = Design Pattern+Library

### 왜 Spring Framework 인가?
- 생산성
- 품질
- 사실상 표준인 (Defacto) Java 프레임워크
- 엔터프라이즈 애플리케이션에 적합한 Java 프레임워크
    - 엔터프라이즈 애플리케이션이란?
    - Spark framework
    - Vert.x framework
    - Play framework
    - Netty framework
    
## Spring Framework 기능

### EJB와 Spring Framework-01
- Yann caroff : co-founder of Spring framework
 - "Spring: a new beginng after the "winter" of traditional J2EE"

### EJB와 Spring Framework-02
- 비침투적(non-intrusive) 인 프레임워크
    - 내가 작성한 비지니스 로직과 떨어져있기 때문에 
- POJO(Plain Old Java Object)
- 오픈소스
- 경량(Light weight) 솔루션
- 모듈러 (Modular) 프레임워크

### EJB와 Spring Framework-03
- Dependency injection
- AOP
- POJO
- Portable service abstractions

### Spring Framework 특징-01
- 경량 컨테이너로서, Spring Bean 을 직접 관리한다.
    - Spring Bean 객체의 라이프 사이클을 관리한다.
    - **Container** Spring Bean 객체의 생성, 보관, 제거에 관한 모든일을 처리한다.
- POJO(Plain Old Java Object) 기반의 프레임워크
    - 일반적인 J2EE 프레임워크와 비교하여, 특정한 인터페이스를 구현하거나 상속을 받을 필요가 없다.
    - 기존에 존재하는 라이브러리를 사용하기 편리하다.
    
### Spring Framework 특징-02
- 제어반전(IoC: Inversion of Control)
    - 의존성 주입(DI: Dependency Injection) 방법 사용
    - 낮은 결합도로 인한 DDD, TDD 와 같은 프로그래밍 개발론에도 적합함
- 관점 지향 프로그래밍(AOP: Aspect-Oriented Programming) 을 지원
    - 복잡한 비지니스 영역의 문제와 공통된 지원 영역의 문제를 분리 할 수있음
    - 문제 해결을 위한 집중
    - 예를 들면 Transaction, Logging, Security and etc
- 영속성과 관련된 다양한 서비스 지원
    - 에를들면 MyBatis, Hibernate, JdbTemplate 등등
- 높은 확장성 및 범용성 그리고 Eco System

### Spring Framework 모듈과 & 프로젝트

스프링 부트와 스프링 프레임워크는 무엇이 틀린가?
- 스프링부트는 auto configuration 지원,  설정을 안하더라도 바로 개발을 시작할 수 있음 
- 스프링부트도 마찬가지로 내부에 코어가있음, 한 껍질 감싸놓은것 본질적으로는 스프링 위에서 동작하는거임


스프링 빈은 스프링 컨테이너에서 관리되는 자바 객체임 

스프링 빈은 커네이너 에서 관리되는 객체 다시 말하면 생성을 하고 파기하고 조합한다.


스프링 빈 생성방법 두가지 방법이있음
- 애플리케이션 설정하기위해 사용하는 스프링빈
- 비지니스 로직을 처리하는 스프링빈


ApplicationContext
컴포넌트 스캔으로 빈 스캐닝을 한다 

빈 주입방법
- field
- setter
- Constructor

Field injection
- @Autowired 기본은 byType 
- @Qualifier - byName

타입이 같아도 변수 이름이 다르면 applicationContext 가 알아서 주입해줌 

Setter injection
- Setter 메소드 위에다가 @Autowired 붙여주면됨
 만약에 이름이 없으면 세터 변수의 이름으로 알아서 주입해줌

Constructor Injection
- 생성자가 하나면 @Autowired 생략해도됨
- 권장하는 방식 : 이유는 단위테스트 하기 편함


Bean Scope
- 스프링 빈은 기본적으로 스코프가 싱글톤임

싱글톤 객체일 경우 같은 객체를 주입해줌
프로토타입 각각 다른객체를 주입


멀티스레드 환경에서 스레드 세이프 하게 해야함
- HashMap 말고 스레드를 보장해주는 맵을 사용해야함
- 싱글톤으로할거면 객체를 따로빼서?


@PostConstruct /@PreDestory

@Primary


실습 todo 로 표시해놓음

Environment 추상화

PSA 서비스 추상화

## 트랜잭션
isolation level 어플리케이션 성능에 많이 미침
데이터의 무결성을 제일 높게해주는거 SERIALIZABLE(8)


propagation 클래스 내에서 쓸건지 새로만들건지

persistent 영역이 바꼈음에도 똑같음 
-> 구현 기술을 갈아 끼워도 스프링 프레임 워크 내에서는 변경범위를 최소화 할수 있음
추상화 해줘서 일반화 해줘서 구현체가 바뀌더라도 코드를 최대한 수정안할수 있도록

## AOP 사용의 예
- Transaction Management
- Cache Abstraction

Weaving
- 코드가 삽입되는 과정을 말함 


@Transactional
- @Controller, @Service, @Repository, @Component 에서만 작동함!!!!!!!!!!
- 대상 객체가 스프링 빈이여야지만 적용됨 이유는 @Transaction은 AOP
- private 메소드 위에다가 @Transactional 적용하면 정상적으로 위빙하지 않음 이유는 프라이빗!!! 숨겨져있기떄문에 위빙못함

    
ProceedingJoinPoint
- Around Advice 에서 사용할 수 있음 

AOP5 브랜치에있음
@Annotation 을 이용한 AOP
- 제일추천함!!!!!!!!!!!!!!!!!!! 인수인계받거나 신입사원 너무 어려움 
- 어노테이션을 이용한 AOP 좋음
