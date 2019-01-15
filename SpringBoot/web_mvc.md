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
 

## 참고사이트
  - [HttpMessageConverters](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#mvc-config-message-converters)