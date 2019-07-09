## 예외처리 
- Controller 레벨에서 처리
- Global 레벨에서 처리 
- HandlerExceptionResolver 이용한 처리

## Controller 레벨에서 처리
- Spring 에서는 Controller에서 발생한 예외에 대해 common 하게 처리 할 수 있는 기능
- @ExceptionHandler 어노테이션을 통해 controlelr 메소드에서 throw 된 Exception 에 대한 공통적인 처리를 할수 있도록 지원

## Global 레벨에서 처리
- 전역으로 처리하도록 지원한다
- @ControllerAdvice : Exception 처리 후 Error Page 등을 통해 처리가 가능하다.
- @RestControllerAdvice
    - REST API 에 대한 Exception 처리등에 용이
    - @RestControllerAdvice = @ControllerAdvice + @ResponseBody
    
- 위 두개의 어노테이션을 이용하여 Web Application 전역으로 @ExceptionHandler 를 사용 **@ControllerAdvice 로 케어가능한 범위는 Dispatcher Servlet 내에서만**

~~~ java
@Slf4j
@ControllerAdvice
public class DemoControllerAdvisor{

  @ExceptionHandler(value = DemoException.class)
  public String handleDemoExceptionForGlobal(DemoException e){
    log.error(e.getMessage());
    return "/error/404";
  }

}
~~~

## e.printStackTrace() 보다는 로거 사용

~~~java
try {
    process();
}catch (IOException e) {
    e.printStackTrace();
}
~~~

~~~java
try{
    process();
}catch(IOException e){
    log.error("fail to process file", e);
}
~~~
- Tomcat 에서 e.printStackTrace() 콘솔로 찍힌 값은 {TOMCAT_HOME}/logs/catalina.out 에만 남음
- 로깅 프레임워크를 이용하면 파일을 쪼개는 정책을 설정 할 수 있음, 여러 서버의 로그를 한곳에서 모아보는 시스템을 활용할수 있음
- log.error() 메서드에서 Exception 객체를 직접 넘기는 e.printStackTrace() 처럼 Exception 에 스택을 모두 남겨줌
- **에러에 추적성을 높이기 위해서 e.toString() 이나 e.getMessage() 로 마지막 메세지만 남기기 보다는 전체 에러 스택을 다 넘기는 편이 좋음**

## 참고사이트
- [Spring Handle Exception](https://jaehun2841.github.io/2018/08/30/2018-08-25-spring-mvc-handle-exception/#%EB%93%A4%EC%96%B4%EA%B0%80%EB%A9%B0)
- [Exception 처리](https://www.slipp.net/questions/350)