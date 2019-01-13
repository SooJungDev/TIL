## 스프링 부트 기본 로거 설정
로깅 퍼사드 vs 로거
- **Commons Logging**(스프링부트에서 사용), SLF4j(심플하고 안전한편)  
  => (추상화 해논 인터페이스들)
- JUL, LOG4J2, **Logback** 
  => 결국에 최종적으로는 Logback 을 사용

스프링 5 로거 관련 변경사항
- [해당 링크](https://docs.spring.io/spring/docs/5.0.0.RC3/spring-framework-reference/overview.html#overview-logging)

- Spring-JCL
 - Commons Logging -> SLF4j or Log4j2
 - pom.xml에 exclusion 안해도 됨
 
 스프링부트 로깅
 - 기본포맷
 - --debug(일부 핵심 라이브러리만 디버깅 모드로)
 - --trace(전부 다 디버깅 모드로)
 - 컬러 출력: spring.output.ansi.enabled = always
 - 파일 출력: logging.file 또는 logging path
 - 로그 레벨 조정: logging.level.패키지 = 로그레벨
 
ex) application.properties 
~~~
spring.output.ansi.enabled=always
logging.path =logs
logging.level.me.crystal.springbootstudy=DEBUG
~~~

## 로깅 커스터마이징
[커스텀 로그 설정 파일 사용하기](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-logging.html)
- Logback: logback-spring.xml
- Log4j2:log4j2-spring.xml
- JUL(비추):logging.properties
- Logback extension
 - 프로파일 <springProfile name="프로파일">
 - Environment 프로퍼티 <springProperty>
- [로거를 Log4j2로 변경하기](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-logging.html#howto-configure-log4j-for-logging)

## 테스트
시작은 일단 spring-boot-starter-test 추가하는 것 부터
- test 스콥으로 추가
@SpringBootTest
- @RunWith(SpringRunner.class)랑 같이써야함
- 빈 설정 파일은 설정을 안해줘도되냐?.. 알아서 찾음 (@SpringBootApplication)
- webEnvironment
 - MOCK : mock servlet environment: 내장 톰캣 구동안함
 - RANDOM_PORT, DEFINED_PORT: 내장 톰캣 사용합.
 - NONE: 서블릿 환경 제공안함.
 
 @MockBean
 - ApplicationContext에 들어있는 빈을 Mock으로 만든 객체로 교체함
 - 모든 @Test 마다 자동으로 리셋
 
 슬라이스테스트
 - 레이어 별로 잘라서 테스트 하고싶을떄
 - @JsonTest
 - @WebMvcTest
 - @WebFluxTest
 - @DataJpaTest
 
 테스트 유틸
 - OutputCapture
 - TestPropertyValues
 - TestRestTemplate
 - ConfigFileApplicationContextInitializer
 
 ## Spring-Boot-Devtools
 - 캐시 설정을 개발 환경에 맞게 변경
 - 클래스패스에 있는 파일이 변경될때마다 자동으로 재시작
  - 직접 껏다켜는거 (cold starts)보다 빠르다. 왜?
  - 릴로딩보다는 느리다(JRebel같은건 아님)
  - 리스타트하고싶지 않은 리소스는? spring.devtools.restart.exclude
  - 리스타트 기능 끄려면? spring.devtools.restart.enabled=false
 - 라이브 릴로드? 리스타트 했을 때 브라우저 자동 리프레시 하는 기능
  - 브라우저 플러그인 설치해야함
  - 라이브 릴로드 서버 끄려면? spring.devtools.liveload.enabled= false
 - 글로벌 설정
  - ~/.spring-boot-devtools.properties
 - 리모트 애플리케이션

## 참고사이트
  - [인프런 강의 스프링부트 로깅1부](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/%EB%A1%9C%EA%B9%85-1%EB%B6%80/)
