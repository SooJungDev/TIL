## RestTemplate
- Blocking I/O 기반의 Synchronous API
- RestTemplateAutoConfiguration
- 프로젝트에 spring-web 모듈이 있다면 RestTemplate.Builder를 빈으로 등록해줌
- [Springboot Rest Endpoints](https://docs.spring.io/spring/docs/current/spring-framework-reference/integration.html#rest-client-access)

- 기본적으로 java.net.HttpURLConnection 사용
- 커스터마이징
 - 로컬 커스터마이징
 - 글로벌 커스터마이징
  - RestTemmplateCustomizer
  - 빈 재정의 

## WebClient
- 의존성 추가 spring-boot-stater-webflux
- Non-Blocking I/O 기반의 Asynchronous API
- WebClientAutoConfiguration
- 프로젝트 spring-webflux 모듈이 있다면 WebClient.Builder를 빈으로 등록해줌
- [WebClient](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-client)

- 기본적으로 Reactor Netty의 HTTP 클라이언트 사용.
- 커스터마이징
 - 로컬 커스터마이징
 - 글로벌 커스터마이징
  - WebClientCustomizer
  - 빈 재정의 



