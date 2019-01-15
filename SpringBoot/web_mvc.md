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

## 참고사이트
  - [HttpMessageConverters](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#mvc-config-message-converters)