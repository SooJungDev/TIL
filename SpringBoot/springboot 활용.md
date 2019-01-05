## SpringApplication
- ApplicationEvent 등록 : ApplicationContext를 만들기 전에 사용하는 리스너는 @Bean으로 등록할 수 없음
  - SpringApplication.addListners()
- WebApplication Type 설정
 - type에는 NONE, SERVLET, REACTIVE 세가지가 있음
 - 서블릿이랑 웹플럭스 같이있는경우 무조건 서블릿 설정을 따라감
 - 따로 설정해줘야지 됨  app.setApplicationType(WebApplication.REACTIVE);
- Application argument 사용하기
  - ApplicationArguments를 빈으로 등록해 주니까 가져다가 쓰면됨
- 애플리케이션 실행한뒤 뭔가 실행하고싶을때
 - ApplicationRunner 추천 
 - 순서 지정 가능@Order (ApplicationRunner가 여러개면) 숫자가 낮은게 먼저들어옴

## 외부설정

사용 할 수 있는 외부설정
- properties
- YAML
- 환경 변수
- 커맨드 라인 아규먼트

프로퍼티 우선순위
- 유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties
- 테스트에 있는 @TestPropertySource
- 커맨드 라인 아규먼트
- SPRING_APPLICATION_JSON (환경변수 또는 시스템 프로티)에 들어있는 프로퍼티
- ServletConfig 파라미터
- ServletContext 파라미터
- javaLcomp/env JNDI 애트리뷰트
- System.getProperties() 자바 시스템 프로퍼티
- OS 환경변수
- RandomValuePropertySource
- JAR 밖에 있는 특정 프로파일용 application properties
- JAR 안에 있는 특정 프로파일용 application properties
- JAR 밖에 있는 application properties
- JAR 안에 있는 application properties
- @PropertySource
- 기본 프로퍼티(SpringApplication.setDefaultProperties)

application.properties 우선순위(높은게 낮은걸 덮어 씁니다.)
1. file:./config/
2. file:./
3. classpath:/config/
4. casspath:

랜던값 설정하기
-${random.*}
플레이스 홀더
- name = soojung
- fullName =${name} choi

타입세이프 프로퍼티 @ConfigurationProperties
- 여러 프로퍼티를 묶어서 읽어 올 수 있음
- 빈으로 등록해서 다른빈에 주입할 수 있음
 - @EnableConfigurationProperties
 - @Component
 - @Bean
 
- 융통성 있는 바인딩
 - context-path(케밥)
 - context_path(언드스코어)
 - contextPath(캐멀)
 - CONTEXTPATH

- 프로퍼티 타입 컨버젼
 - @DurationUnit
- 프로퍼티 값 검증
 - @Validated
 - JSR-303(@NotNull,)
- 메타정보 생성
- @Value
 - SpEL 을사용할 수있지만 위에있는 기능들 전부 사용하지 못함
 
## 프로파일 
@Profile 어노테이션은 어디에
- @Configuration
- @Component

어떤 프로파일을 활성화 할 것인가?
- spring.profiles.active

어떤 프로파일을 추가 할 것인가?
- spring.profile.include

프로파일용 프로퍼티
- application-{profile}.properties
 
## 참고사이트
  - [인프런 백기선님 강의!](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/springapplication-1%EB%B6%80/)
  - [외부설정 파일](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config)