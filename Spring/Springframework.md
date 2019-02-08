## Inversion of Control (IoC)
- 제어의 역
- 쉽게 말해서 쓸 곳에서 만들지 않고 누군가 알아서 줌
- 작성한 코드를 Framework 가 컨트롤 한다.

IoC가 의미하는것
- 객체(Bean) 생명주기 관리(컨트롤)
- DI pattern 제공
- 비지니스 로직에 집중

## Ioc (Inversion of Control) container
- ApplicationContext(BeanFactory)
- 빈(Bean)을 만들고 엮어 주며 제공해줌
- 빈설정
    - 이름 또는 ID
    - 타입
    - 스코프
- 아이러니하게도 컨테이너르르 직접 쓸일은 많지 않음

## Bean
- 스프링 IoC 컨텡너가 관리하는 객체

어떻게 등록하지?
- Component Scanning
    - @Component
        - @Repository
        - @Service
        - @Controller
- 또는 직접 일일히 XML이나 자바 설정 파일에 등록

어떻게 꺼내쓰지?
- @Autowired 또는 @Inject
- 또는 ApplicationContext에서 getBean()으로 직접 꺼내거나

특징
- 오로지 "빈" 들만 의존성 주입(Dependency Injection)을 해줍니다

## Dependency Injection (의존성 주입)
- 필요한 의존성을 어떻게 받아올것인가?
- @Autowired / @Inject를 어디에 붙일까?
    - 생성자
    - 필드
    - Setter
    
 1. Contructor Injection 
 - Spring 4.3에서 단일 생성자인 경우 @Autowired를 붙일 필요없음
 ~~~ java
 @Component
 public class ConstructorInjection{
   
    private final LoginService loginService;
    privage final SignupService signupService;
    
    @Autowired // => 생략가능
    public ConstructorInjection(LoginService loginService, SignupService signupService){
        this.loginService = loginService;
        this.signupService = signupService;
    }
 
 }
 ~~~  
- lombok 으로 의존성 주입하는 법
    -  @AllArgConstructor 애노테이션만 달아주면됨
 ~~~ java
 @AllArgConstructor
 @Component
 public class ConstructorInjection{
   
    private final LoginService loginService;
    privage final SignupService signupService;
 
 }
 ~~~ 

 
2. Field Injection
~~~java 
@Component
public class FieldInjection{
   @Autowired
   private LoginService loginService;
   @Autowired
   private SingupService signupService;

}
~~~

3.Setter Injection
~~~java
@component
public class SetterInjection {
   private LoginService loginService;
   private SignupService signupService;
   
   
   @Autowired
   public void setLoginService(LoginService loginService){
     this.loginService = loginService;
   }
   
   @Autowired
   public void setSignupService(SignupService signupService){
     this.signupService = signupService;
   }

}
~~~

## Constructor Injection을 권장하는 이유
1. 단일 책임의 원칙
- 생성자의 인자가 많을 경우 코드량도 코드량도 많아지고, 의존관계도 많아져 단일 책임의 원칙에 위배됨
- 그래서 Constructor Injection 사용함으로써 의존관계, 복잡성을 쉽게 알 수있어 리팩토링의 단초를 제공하게됨


2. 테스트 용이성
- DI 컨테이너에서 관리되는 클래스는 특정 DI 컨테이너에 의존하지 않고 POJO여야 함
- DI 컨테이너를 사용하지 않고도 인스턴스화 할수 있고, 단위테스트 가능, 다른 DI 프레임워크로 전환 할수도 있음


3. Immutability(불변성)
- Constructor Injection 에서는 필드를 final로 선언할수있음.
- 불변 객체가 가능한데 비해 Field Injection은 final로 선언 할 수 없기 때문에 객쳋가 변경 가능한 상태가 됨

4. 순환 의존성
- 멤버 객체가 순환 의존성을 가질 경우 BeanCurrentlyInCreationException이 발생해서 순환 의존성을 알수 있음

5. 의존성 명시
- 의존 객체 중 필수는 Constructor Injection을 옵션 인 경우 Setter Injection 을 활용 할 수 있다.


## AOP
- 쉽게 말해 흩어진 코드를 한곳으로 모아주는 역할
- 공통으로 겹치는 부분을 없앨 수 있음

## AOP 적용 예제
- @LogExecutionTime 으로 메소드 처리 시간 로깅하기

- @LogExecutionTime 애노테이션 (어디에 적용할지 표시 해주는 용도)
~~~ java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime{

}
~~~

실제 Aspect (@LogExecutionTime 애노테이션 달린곳에 적용)
~~~ java
@Component
@Aspect
public class LogAspect{

    Logger logger = LoggerFactory.getLogger(LogAspect.class);
    
    ​@Around​(​"@annotation(LogExecutionTime)"​)
    ​public ​Object ​logExecutionTime​(ProceedingJoinPoint joinPoint) ​throws ​Throwable {
         StopWatch stopWatch = ​new ​StopWatch()​; 
         ​stopWatch.start()​;
    ​     Object proceed = joinPoint.proceed()​; 
         ​stopWatch.stop()​;
    ​     logger​.info(stopWatch.prettyPrint())​; return ​proceed​;
    ​
    }

}
~~~

## PSA
- 잘만든 인터페이스
- Portable Service Abstraction 
- 스프링이 제공하는 대표적인 기술이 바로 일관성 있는 서비스 추상화 기술

예제
- 스프링 트랜잭션
    - 나의 코드 : @Transactional 
- 스프링 캐시
    - 나의 코드 : @Cacheable | @CacheEvict
- 스프링 웹 MVC
    - 나의 코드 : @Controller | @RequestMapping



## 참고사이트
  - [스프링 프레임워크 입문](https://www.inflearn.com/course/spring/)
  - [spring - 1주차 IoC와 DI](https://www.slipp.net/wiki/pages/viewpage.action?pageId=25527606)
  - [DI(의존성 주입)가 필요한 이유와 Spring에서 Field Injection보다 Constructor Injection이 권장되는 이유](http://www.mimul.com/pebble/default/2018/03/30/1522386129211.html)
  
