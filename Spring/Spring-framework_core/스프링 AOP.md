## AOP: 개념소개
- Aspect-Oriendted Programming(AOP)
- OOP를 보완하는 수단으로 흩어진 Aspect을 모듈화 할 수 있는 프로그래밍 기법

AOP 주요개념
- Aspect(각각의 모듈)
- Target(적용이 되는 대상)
- Advice(해야할 일들)
- Join point(실행시점 끼어들수있는 지점, 스펙에 가까움)
- Pointcut(어디에 적용해야되는지 정보를 들고있음)

AOP 구현체
- [AOP](https://en.wikipedia.org/wiki/Aspect-oriented_programming)
- 자바
    - AspectJ
    - 스프링 AOP
 
AOP 적용방법
- 컴파일 
    - 컴파일 시점에 삽입해줌
- 로드타임
   - 컴파일은 순수하게 함,A라는 클래스 파일을 로딩하는 시점에 Aspect 코드를 삽입함  
=> Aspect J 위에 두가지 방법 사용할때 주로 씀  
- 런타임
    - A 클래스 타입의  bean을 만들 때 A타입에 프록시 빈을 만듬
    - 프록시 빈이 A가 가진 메소드를 실행하기전에 hello (Aspect이 가진 메소드를) 실행함  
=> 스프링 AOP 에서 주로사용

## 프록시 기반 AOP   
스프링 AOP 특징
- 프록시 기반의 AOP 구현체
- 스프링 빈에만 AOP를 적용 할 수 있다.
- 모든 AOP 기능을 제공하는것이 목적이 아니라, 스프링 IOC와 연동하여 엔터프라이즈 애플리케이션에서 가장   
흔한 문제에 대한 해결책을 제공하는것이 목적

프록시 패턴
- 왜? (기존 코드 변경 없이) 접근제어 또는 부가기능 추가
- 기존 코드를 건드리지 않고 성능을 측정해보자(프록시 패턴으로)

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
        - 조합이 잘되지 않아서 불편함
    - @annotation
        - 좀더 편함 (일일히 붙여줘야되지만..) 
    - bean
        - 빈이 가지고 있는 모든 메소드에 적용됨
- 포인트컷 조합
    - &&,||,!
    
 
 어드바이스 정의
 - @Before(이전)
    - 어드바이스 타켓 메소드가 호출되기전에 어드바이스 기능을 수행
 - @After(이후)
    - 타겟 메소드의 결과에 관계없이(즉 성공, 예외 관계없이) 타켓 메소드가 완료되면 어드바이스 기능을 수행    
 - @AfterReturning(정상적 반환 이후)
    - 타켓 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행
 - @AfterThrowing(예외 발생 이후)
    - 타켓 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행
 - @Around(메소드 실행 전후)
    - 어드바이스가 타겟 메소드를 감싸서 타켓 메소드 호출전과 후에 어드바이스 기능을 수행
 
 - [aop-pointcuts](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-pointcuts)

## 참고
- [AOP 정리 (3)](https://jojoldu.tistory.com/71)
