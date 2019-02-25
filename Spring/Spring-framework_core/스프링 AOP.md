## AOP: 개념소개
- Aspect-Oriendted Programming(AOP)
- OOP를 보완하는 수단으로 흩어진 Aspect을 모듈화 할 수 있는 프로그래밍 기법

AOP 주요개념
- Aspect과 Target
- Advice
- Join point와 Poincut

AOP 구현체
- [AOP](https://en.wikipedia.org/wiki/Aspect-oriented_programming)
- 자바
    - AspectJ
    - 스프링 AOP
 
AOP 적용방법
- 컴파일 
- 로드타임
- 런타임

## 프록시 기반 AOP   
스프링 AOP 특징
- 프록시 기반의 AOP 구현체
- 스프링 빈에만 AOP를 적용 할 수 있다.
- 모든 AOP 기능을 제공하는것이 목적이 아니라, 스프링 IOC와 연동하여 엔터프라이즈 애플리케이션에서 가장   
흔한 문제에 대한 해결책을 제공하는것이 목적

프록시 패턴
- 왜? (기존 코드 변경 없이) 접근제어 또는 부가기능 추가
- 기존 코드를 건드리지 않고 성능을 측정해보자(프록시패턴으로)

문제점  
- 매번 프록시 클래스를 작성해야 하는가?
- 여러 클래스 여러 메소드에 적용하려면?
- 객체들 관계도 복잡하고..


그래서 등장한 것이 스프링 AOP
- 스프링 IOC 컨테이너가 제공하는 기반 시설과 Dynamic 프록시를 사용하여 여러 복잡한 문제 해결
- 동적 프록시: 동적으로 프록시 객체 생성하는 방법
    - 자바가 제공하는 방법은 인터페이스 기반 프록시 생성
    - CGlib은 클래스 기반 프록시도 지원

- 스프링 IoC: 기존 빈을 대체하는 동적 프록시 빈을 만들어 등록시켜준다.
    - 클라이언트 코드 변경 없음.
    - [AbstractAutoProxyCreator](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/aop/framework/autoproxy/AbstractAutoProxyCreator.html) implements [BeanPostProcessor](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html) 
    

## @AOP
애노테이션 기반의 스프링 @AOP

의존성 추가
~~~
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
~~~

애스팩트 정의
- @Aspect
- 빈으로 등록해야 하니까(컴포넌트 스캔을 사용한다면) @Component도 추가

포인트 컷 정의
- @Pointcut(표션식)
- 주요 표현식
    - execution
    - @annotation
    - bean
- 포인트컷 조합
    - &&,||,!
    
 
 어드바이스 정의
 - @Before
 - @AfterReturning
 - @AfterThrowing
 - @Around
 
 - [aop-pointcuts](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-pointcuts)