## @Async
- @Async 어노테이션을 빈에 넣으면 별도의 스레드에서 실행되는것. 이를테면 호출자는 호출된 메소드가 완료될때까지 기다릴필요없음

## Async 기능 켜기
자바 설정 @Configuration으로 비동기를 쓰려면 간단한 설정 클래스에 @EnableAsync를 추가해 주기만 하면됨
~~~ java
@Configuration
@EnableAsync
public class SpringAsyncConfig{
....
} 
~~~
몇가지 옵션 또한 설정해 줄 수 있음
- @EnableAsync은 스프링 @Async 어노테이션 감지  이옵션으로 사용자 정의된 다른 어노테이션 또한 감지
- mode : 사용해야될 advice 타입을 가르킨다. 
- proxyTargetClass: 사용해야할 proxy 타입을 가르킨다.
- order : 적용해야할 순서를 설정 , 단지 모든 현존의 프록시를 고려하기위해 기본값으로 마지막부터 실행됨

## @Async 어노테이션 
@Async 두가지 제약사항
1. public 메소드에만 적용해야함
2. 셀프호출 같은 클래스안에 async 메소드를 호출하면 작동하지 않음
**그 이유는 간단한데 메소드가 public이어야 프록시가 될수 있기 때문이고, 셀프호출은 프록시를 우회하고 해당 메소드를 직접 호출하기 때문에 작동하지 않음**

## 리턴 타입이 없는 메소드 
- void 일 경우 메소드가 비동기로 작동함
~~~ java
@Async
public void asyncMethodWithVoidReturnType(){
 System.out.print("Execute method asynchronously"+ Thread.currentThread().getName());
}
~~~

## 리턴 타입이 있는 메소드
Future 객체에 실제 리턴값을 넣음으로서 @Async는 리턴타입이 있는 메소드에 적용 할 수 있음
~~~java
@Async
public Future<String> asyncMethodWithReturnType(){
 system.out.println("Execute method asynchronously"+ Thread.currentThread().getName());
 try{
    Thread.sleep(5000);
    return new AsyncResult<String>("hello world!!!!");
  }catch(InterruptedExcetion){
    //   
  }
  return null;
}
~~~
- 스프링은 또한 Future를 구현한 AsyncResult 클래스를 제공하며 이는 비동기 메소드 실행의 결과를 가져오는데 사용
이제 위의 메소드를 호출하여 Future 객체를 사용해 비동기 처리 결과값을 가져오기

~~~java
public void testAsyncAnnotationForMethodsWithReturnType()
 throws interruptedException, ExcuetionException{
  System.out.println("invoiking an asynchronous method."+Thread.currentThread().getName));
  Future<String> future = asyncAnnotationExample.asyncMethodWithReturnType();
  
  while(true){
   if(future.isDone()){
     system.out.println("Result from asynchronous process"+future.get());
     break;
   }
   System.out.println("Continue doing something else");
   Thread.sleep(1000);
  }
}

~~~


## 참고사이트
  - [스프링에서 @Async로 비동기처리하기](http://springboot.tistory.com/38)
