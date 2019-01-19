## 스프링 웹 MVC
- [스프링 웹 MVC](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#spring-web)
- 스프링부트 MVC
  - 자동 설정으로 제공하는 여러 기본 기능
- 스프링 MVC 확장
 - @Configuration + WebMvcConfigurer
- 스프링 MVC 재정의
 - @Configuration+@EnableWebMvc

## HttpMessageConverters
- HTTP 요청 본문을 객체로 변경하거나 객체를 HTTP 응답 본문으로 변경할때 사용
  ex) {"username": "soojung", "password":"1234"}  <-> User
- @RequestBody
- @ResponseBody 
  - @RestController 에서는 @ResponseBody을 생략해도됨 자동으로 알아서 해줌
  - @Controller 를 쓸 경우 @ResponseBody 꼭 써줘야 메세지 컨버터가 적용됨

~~~java
@RestController
public class UserController {

    @GetMapping("/hello")
    public String hello(){
        return "hello";
    }

    @PostMapping("/user")
    public @ResponseBody User create(@RequestBody User user){
        return  null;
    }
}
~~~

## ViewResolve
- 뷰 리졸버 설정 제공
- HttpMessageConvertersAutoConfiguration

XML 메세지 컨버터 추가하기
~~~
<dependency>
   <groupId>com.fasterxml.jackson.dataformat</groupId>
   <artifactId>jackson-dataformat-xml</artifactId>
   <version>2.9.6</version>
</dependency>
~~~

## 정적 리소스 지원
- 기본 리소스 위치
 - classpath:/static
 - classpath:/public
 - classpath:/resources/
 - classpath:/META-INF/resources
 - 예) "/hello.html" => /static/hello.html
 - spring.mvc.static-path-pattern: 맵핑 설정 변경 가능
 - spring.mvc.static-locations: 리소스 찾을 위치 변경 가능
- Last-Modified 헤더를 보고 304 응답을 보냄
- ResourceHttpRequestHandler 가 처리함
 - WebMvcConfigurer 의 addRersouceHandlers로 커스터마이징 할수 있음
 
 ~~~ java
 public class WebConfig implements WebMvcConfigurer {
     @Override
     public void addResourceHandlers(ResourceHandlerRegistry registry) {
         registry.addResourceHandler("/m/**")
                 .addResourceLocations("classpath:/m/") // 끝날떄 / 반드시해줘야됨 안하면 잘 매핑안됨
                 .setCachePeriod(20);
     }
 }
 ~~~

## 웹 jar
웹JAR 맵핑 “ /webjars/**”
- 버전 생략하고 사용하려면
 - webjars-locator-core 의존성 추가 (pom.xml에 추가해줌)
 
 ~~~html
 <script src="/webjars/jquery/dist/jquery.min.js"></script>
 <script>
    $(function() {
        console.log("ready!");
    });
 </script>
 ~~~
 
 ## index 페이지와 파비콘
 웰컴페이지
 - index.html 찾아보고 있으면 제공
 - index. 템플릿 찾아보고 있으면 제공
 - 둘다없으면 에러페이지
 
 파비콘
 - favicon.ico
 - [파비콘만들기](https://favicon.io/)
 - [파비콘이 안바뀔때](https://stackoverflow.com/questions/2208933/how-do-i-force-a-favicon-refresh)
 
 ## Thymeleaf
 스프링 부트가 자동 설정을 지원하는 템플릿 엔진
 - FreeMarker
 - Groovy
 - Thymeleaf
 - Mustache
 
 [jsp를 권장하지 않는 이유](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-jsp-limitations)
 - JAR 패키징 할때는 동작하지 않고, WAR 패키징 해야함
 - Undertow는 JSP를 지원하지 않음
 
 Thymeleaf 사용하기
 - [Thymeleaf](https://www.thymeleaf.org/)
 - [Thymeleaf 5분안에 익히기](https://www.thymeleaf.org/doc/articles/standarddialect5minutes.html)
 - 의존성 추가: spring-boot-starter-thymeleaf
 - 템플릿 파일 위치:/src/main/resources/template/
 - [예제](https://github.com/thymeleaf/thymeleafexamples-stsm/blob/3.0-master/src/main/webapp/WEB-INF/templates/seedstartermng.html)
 
 ## HtmlUnit
 - Html 템플릿 뷰 테스트를 보다 전문적으로 하자
 - [htmlunit](http://htmlunit.sourceforge.net/)
 - [htmlunit 시작하기](http://htmlunit.sourceforge.net/gettingStarted.html)
 
 ~~~java
    @Autowired
     WebClient webClient;
     
     @Test
     public void hello() throws Exception {
         // 요청 "/hello"
         // 응답
        HtmlPage page = webClient.getPage("/hello");
        HtmlHeading1 h1 = page.getFirstByXPath("//h1");
        assertThat(h1.getTextContent()).isEqualToIgnoringCase("soojung");
     }
 ~~~
 
 ## ExceptionHandler
 스프링 @MVC 예외 처리 방법
 - @ControllerAdvice
 - @ExceptionHandler
 
 스프링 부트가 제공하는 기본 예외 처리기
 - BasicErrorController
  - Html과 json 응답 지원
 - 커스터마이징 방법
  - ErrorController 구현
 
 커스텀 에러 페이지
 - 상태코드 값에 따라 에러 페이지 보여주기
 - src/main/resources/static|template/error/
 - 404.html
 - 5XX.html
 - ErrorViewResolver 구현
 
 ## Spring HATEOAS
 Hypermedia As The Engine Of Application State
 - 서버: 현재 리소스와 연관된 링크정보를 클라이언트에게 제공한다.
 - 클라이언트: 연관된 링크정보를 바탕으로 리소스에 접근한다.
 - 연관된 링크정보
  - Relation
  - Hypertext Reference
 - spring-boot-stater-hateoas 의존성추가
 - [Understanding HATEOAS](https://spring.io/understanding/HATEOAS)
 - [rest-hateoas guides](https://spring.io/guides/gs/rest-hateoas/)
 - [HATEOAS - Reference](https://docs.spring.io/spring-hateoas/docs/current/reference/html/)
 
 ObjectMapper 제공
 - spring.jackson.*
 - Jackson2ObjectMapperBuilder
 
 LinkDiscovers 제공
 - 클라이언트 쪽에서 링크 정보를 Rel 이름으로 찾을때 사용 할 수있는 XPath 확장클래스
 
 ## CORS
 - Single-Origin Policy
 - Cross-Origin Resource Sharing
 - Origin? 하나의 오리진
  - url 스키마(http, https)
  - hostname(whiteship.me,localhost)
  - 포트(8080,18080)
 
 스프링 MVC @CrossOrigin
 - [CORS](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#mvc-cors)
 - @Controller 나 @RequstMapping 에 추가하거나
 - WebMvcConfigurer 사용해서 글로벌 설정
 
 ~~~java
 @Configuration
 public class WebConfig implements WebMvcConfigurer {
     @Override
     public void addCorsMappings(CorsRegistry registry) {
         registry.addMapping("/**")
                 .allowedOrigins("http://localhost:18080");
     }
 }
 ~~~

## 참고사이트
  - [HttpMessageConverters](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#mvc-config-message-converters)