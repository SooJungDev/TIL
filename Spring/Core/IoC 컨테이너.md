## 스프링 IoC 컨테이너와 빈
Inversion of Control: 의존관계주입(Dependency Injection)이라고도 하며, 어떤 객체가 사용하는   
**의존객체를 직접만들어 사용하는게 아니라 주입받아 사용하는 방법**을 말함.  

스프링 IoC컨테이너
- BeanFactory
- 애플리케이션 컴포넌트의 중앙저장소
- 빈 설정 소스로 부터 빈 정의를 읽어들이고, 빈을 구성하고 제공한다.

빈
- 스프링 IoC 컨테이너가 관리하는 객체
- 장점
    - 의존성 관리
    - 스코프
        - 싱글톤: 하나
        - 프로포토타입: 매번 다른 객체
    - 라이프사이클 인터페이스
    
[ApplicationContext](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html)
- BeanFactory
- 메세지 소스처리 기능(i18n)
- 이벤트 발행기능
- 리소스 로딩 기능
- ...

## ApplicationContext와 다양한 빈 설정 방법
스프링 IoC 컨테이너의 역할
- 빈 인스턴스 생성
- 의존 관계 설정
- 빈 제공

ApplicationContext
- ClassPathXmlApplicationContext(XML)
- AnnotationConfigApplicationContext(Java)

빈설정
- 빈 명세서
- 빈에 대한 정의를 담고있다
    - 이름
    - 클래스
    - 스코프
    - 생성자 아규먼트(constructor)
    - 프로퍼트(setter)
    - ...
    
컴포넌트 스캔
- 설정방법
    - XML 설정에서는 context:component-scan
    - 자바 설정에서 @ComponentScan
- 특정 패키지 이하의 모든 클래스 중에 @Component 애노테이션을 사용한 클래스를 빈으로 자동으로 등록해줌

## @Autowire
- 필요한 의존 객체의 "타입"에 해당하는 빈을 찾아 주입한다.

@Autowired
- required: 기본값은 true(따라서 못찾으면 애플리케이션 구동 실패)

사용할 수 있는 위치
- 생성자(스프링 4.3부터는 생략가능)
- 세터
- 필드    

경우의 수
- 해당 타입의 빈이 없는 경우
- 해당 타입의 빈이 한 개인 경우
- 해당 타입의 빈이 여러 개인 경우
    - 빈 이름으로 시도.
        - 같은 이름의 빈 찾으면 해당 빈 사용
        - 같은 이름 못 찾으면 실패
        

같은 타입의 빈이 여러개 일때
- @Primary
    - 주로 사용하는 빈에 붙여줌 우선으로 주입됨
    - 추천 타입세이프하므로
- 해당 타입의 빈 모두 주입받기
    ~~~ java
     @Autowired
     List<BookRepository> bookRepositories;
    ~~~ 
- @Qualifier (빈 이름으로 주입)
    - @Qualifier("soojungBookRepository") 비추천

동작 원리
- 첫시간에 잠깐 언급했던 빈 라이프 사이클 기억?!?!
- [BeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html)
    - 새로 만든 빈 인스턴스를 수정 할 수있는 라이프 사이클 인터페이스
- [AutowiredAnnotaionBeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html) extends BeanPostProcessor
    - 스프링이 제공하는 @Autowired와 @Value 애노테이션 그리고 JSR-330 의 @Inject 애노테이션을 지원하는 애노테이션 처리기
    
## @Component와 컴포넌트 스캔
컴포넌트 스캔 주요기능
- 스캔 위치 설정
    - basePackages 밑에 있는 애들만 스캔함 그밖에 있으면 스캔하지 않아서 빈이 주입되지 않음 
- 필터: 어떤 애노테이션을 스캔 할지 또는 하지않을지 
    

컴포넌트 스캔하는 대상들  
@Component  
- @Repository
- @Service
- @Controller
- @Configuration

동작원리
- @ComponentScan은 스캔할 패키지와 애노테이션에 대한 정보
- 실제 스캐닝은 [ConfigurationClassPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ConfigurationClassPostProcessor.html)라는 [BeanFactoryPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanFactoryPostProcessor.html)에 의해 처리됨

- @Bean 애노테이션을 사용해서 빈등록

- function을 사용한 빈 등록
    - 애플리케이션 구동시 성능상에 조금 이점이 있을 수 있음 
    - 비추천 불편함.. 설정파일들이 많아 질 수있음 
~~~ java

public static void main(String[] args){
   new SpringApplicationBuilder()
            .sources(Demospring51Application.class)
            .initializers(ApplicationContextInitializer<GenericApplicationContext>)
                applicationContext ->{
                   applicationContext.registerBean(MyBean.class);
                })
                .run(args);
}
~~~

## 빈의 스코프
스코프
- 싱글톤 (아무런 설정을 하지않으면 기본이 싱글톤)
    - 해당 빈의 인스턴스가 오직 한개 뿐이라는말
- 프로토 타입
    - Request
    - Session
    - WebSocket
    - ...
    
프로토타입 빈이 싱글톤 빈을 참조하면?
- 아무 문제 없음

싱글콘 빈이 프로토타입 빈을 참조하면?
- 프로토타입 빈이 업데이트가 안되네?
- 업데이트 하려면
    - scoped-proxy
    - Object-Provider
    - Provider(표준)

- 프로토 타입
    - 빈을 받아 올때마다 매번 다른 새로운 인스턴스를 생성함    
~~~java
@Component
@Scope("prototype")
public class Proto{

    @Autowired
    Single single;  // 별문제 없음

}
~~~   


~~~java
@Component
public class Single{
    
    @Autowired
    private Proto proto;
    
    public Proto getProto(){
        return proto;
    }

}
~~~ 
- 싱글톤 객체 안에 prototype 인게 변경되지 않음


변경이 바뀌게 해주려면 해당 클래스에  proxyMode =ScopedProxyMode.TARGET_CLASS 추가
- INTERFACES 도 사용가능
~~~java
@Component
@Scope("prototype", proxyMode =ScopedProxyMode.TARGET_CLASS)
public class Proto{

}
~~~   

   
[프록시](https://en.wikipedia.org/wiki/Proxy_pattern)
- 프록시로 감싸줘라!!!!
- 직접 참조하지 않고 프록시를 거쳐서 참조해줘라 
- 매번 바꿔 줄 수있는 프록시로 감싸서 참조해라

상글톤 객체 사용 시 주의 할점
- 프로퍼티가 공유 (공유되기 때문에 thread safe 한 코드를 작성해야함)
- ApplicationContext 초기 구동시 인스턴스 생성.

## Environment 1부 프로파일
프로파일과 프로퍼티를 다루는 인터페이스

ApplicationContext extends [EnvironmentCapable](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/env/EnvironmentCapable.html)
- getEnvironment()

프로파일
- 빈들의 그룹
- [Environment](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/env/Environment.html)

프로파일 유즈케이스
- 테스트 환경에서는 A라는 빈을 사용하고, 배포환경에서는 B라는 빈을 쓰고싶다.
- 이 빈은 모니터링 용도니까 테스트 할때는 필요가 없고 배포할때만 등록이되면 좋겠다.

프로파일 정의하기
- 클래스에 정의
    - @Configuration @Profile("test")
    - @Component @Profile("test")
- 메소드에 정의
    - @Bean @Profile("test")

프로파일 설정하기
- @ -Dspring.profiles.active="test,A,B,..."
- @ActiveProfiles(테스트용)

프로파일 표현식
- ! (not)
- & (and)
- | (or)
  - ex) @Profile("!prod & test") 
    - 표현식 간단하게 해주는것이 좋음
    
## Environment 2부 프로퍼티
프로퍼티
- 다양한 방법으로 정의 할 수 있는 설정값
- Environment의 역할은 프로퍼티 소스 설정 및 프로퍼티 값 가져오기
    - Enviroment.getProperty("app.name);

프로퍼티에는 우선순위가 있다.
- StandardServletEnvironment의 우선순위
    - ServletConfig 매개변수
    - ServletContext 매개변수
    - JNDI (java:comp/env/)
    - JVM 시스템 프로퍼티(-Dkey="value")      

@PropertySource
- Environment를 통해 프로퍼티 추가하는 방법

스프링 부트의 외부 설정 참고
~~~java
 @Value("${app.name}")
 String appName;
~~~
- 기본 프로퍼티 소스 지원(application.properties)
- 프로파일까지 고려한 계층형 프로퍼티 우선 순위 제공

## MessageSource
국제화(i18n) 기능을 제공하는 인터페이스

ApplicationContext extends MessageSource
- getMessage(String code, Object[] args, String, default,Locale, Ioc)
- ...

스프링 부트를 사용한다면 별다른 설정 필요없이 messages.properties 사용할 수 있음
- messages.properties
- messages_ko_KR.properties (_바로 이어져야함)
- ...

릴로딩 기능이 있는 메세지 소스 사용하기
~~~java
@Bean 
public MessageSource messageSource(){
  var messageSource = new ReloadableResourceBundleMessageSource();
  messageSource.setBasename("classpath:/messages");
  messageSource.setDefaultEncoding("UTF-8");
  messageSource.setCacheSeconds(3);
  return messageSource;
}
~~~

## ApplicationEventPublisher
- 이벤트 프로그래밍에 필요한 인터페이스 제공.
- [옵져버 패턴](https://en.wikipedia.org/wiki/Observer_pattern) 구현체

ApplicationContext extends [ApplicationEventPublisher](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationEventPublisher.html)
- publishEvent(ApplicationEvent event)

이벤트 만들기
- ApplicationEvent 상송
- 스프링 4.2부터 이클래스를 상속 받지 않아도 이벤트로 사용 할 수 있다.

이벤트 발생 시키는 방법
- ApplicationEventPublisher.publishEvent();

이벤트 처리하는 방법
- ApplicationListener<이벤트> 구현한 클래스 만들어서 빈으로 등록하기.
- 스프링 4.2부터는 [@EventListener](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/event/EventListener.html)를 사용해서 빈의 메소드에서 사용 할 수있다.
- 기본적으로 synchronized
- 순서를 정하고 싶다면 @Order 와 함께 사용
- 비동기적으로 실행하고 싶다면 @Async와 함께 사용
    - 비동기이기때문에 순서는 보장되지 않는다 @Order 는 의미없음..
    - @Async 라고 붙인다고해서 async가 되지 않음
      - @EnableAsync 붙여야함 그래야 작동!

~~~java
@Component
public class MyEventHandler {

    @EventListener
    @Order(Ordered.HIGHEST_PRECEDENCE + 2)
    public void onApplicationEvent(MyEvent event){
        System.out.println("이벤트 받았다. 데이터는"+event.getData());
   }
}
~~~

~~~ java
@Component
public calss AnotherHandler {

    @EventListener
    @Order(Ordered.HIGHEST_PRECEDENCE)
    public void onApplicationEvent(MyEvent event){
        System.out.println("Another"+event.getData());
   }
}
~~~

스프링이 제공하는 기본이벤트
- ContextRefreshedEvent: ApplicationContext 를 초기화 했거나 리프레시 했을때 발생
- ContextStartedEvent: ApplicationContext 를 start() 하여 라이프사이클 빈들이 시작 신호를 받은 시점에 발생
- ContextStoppedEvent: ApplicationContext 를 stop()하여 라이프사이클 빈들이 정지 신호를 받은 시점에 발생
- ContextClosedEvent: ApplicationContext 를 close()하여 싱글톤 빈 소멸되는 시점에 발생
- RequestHandledEvent: HTTP 요청을 처리 했을때 발생.

## ResourceLoader
리소스를 읽어오는 기능을 제공하는 인터페이스

ApplicationContext extends ResourceLoader

리소스 읽어오기
- 파일 시스템에서 읽어오기
- 클래스패스에서 읽어오기
- URL로 읽어오기
- 상대/절대 경로로 읽어오기

Resource getResource(java.lang.String location)

~~~ java
@Component
public class AppRunner implements ApplicationRunner{

    @Autowired
    ResourceLoader resourceLoader;
    
    @Override
    public void run(ApplicationArguments args) throws Exception {
        Resource resource = resourceLoader.getResource("classpath:test.txt");
        System.out.println(resource.exists());
        System.out.println(resource.getDescription());
        System.out.println(Files.readString(Path.of(resource.getURI())));
    }

}
~~~





