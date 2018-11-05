## ResponseEntity 이란?
- @RestController 는 별도의 뷰를 제공하지 않는 형태로 서비스를 실행하기 떄문에 리턴 데이터가 예외적인 상황에서 문제가 발생 할수있음  
웹의 경우 HTTP 상태코드가 이러한 정보를 나타나는데 사용됨
- Spring에서 제공하는 ResponseEntity 타입은 개발자가 직접 결과 데이터와 HTTP 상태코드를 직접 제어 할 수 있는 클래스임
- ResponseEntity를 사용하면 404나 500같은 에러를 전송하고 싶은 데이터와 함께 전송 할 수 있기떄문에 세밀한 제어가 가능해짐

## ResponseEntity 적용하기
~~~ java
   @Setter
   @Getter
   @ToString
   public class ApiResponseMessage{
    // HttpStatus
    private String status;
    // Http Default Message
    private String message;
    //Error code
    private String errorCode;
    
    public ApiResponseMessage(){}
    
    public ApiResponseMessage(String status, String message, String errorCode){
       this.status = status;
       this.message = message;
       this.errorCode =errorCode;
    }
  }
~~~

~~~ java
@GetMapping("/hello")
public ResponseEntity<ApiResponseMessage> helloWorld(){
    ApiResponseMessage message = new ApiResponseMessage("Success","Hello, world","");
    return new ResponseEntity<ApiResponseMessage>(message, HttpStatus.OK);
}
~~~


## 참고사이트
  - [ResponseEntity](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html)
  - [ResponseEntity 타입이란?](https://doublesprogramming.tistory.com/204)
  - [Spring boot로 ResponseEntity 적용하기](https://steemit.com/kr-dev/@igna84/spring-boot-responseentity)