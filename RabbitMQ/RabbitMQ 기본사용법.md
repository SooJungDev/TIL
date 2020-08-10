## Rabbit MQ
- 래빗엠큐는 오픈소스 AMQP 브로커
- AMQP 유선을 통해 전송되는 메시지 형식을 포함하는 개발형 프로토컬

## AMQP 기본사항
- AMQP 기반 생성자는 큐에 직접 게시하지 않고 익스체인지에 게시한다
- 큐가 선언되면 익스체인지에 바인딩돼야한다
- 토픽 익스체인지를 통해 라우팅 키는 comment.* 와 같은 와일드 카드를 사용할 수 있다. 이상황은
사용자가 조건을 제공할때 까지 실제 라우팅 키를 알수 없는 클라이언트에 가장 적합하다.

~~~java
@RabbitListener(bindings = @Queuebinding(
    value = @Queue, 
    exchange = @Exchange(value = "learning-spring-boot"),
    key = "comments.new"
))
public void save(Comment comment){
    commentRepository.save(comment)
}
~~~
- @RabbitListener 어노테이션은 메시지를 사용하는 방법을 등록하는 가장 쉬운 방법
- @QueueBindng 어노테이션은 큐와 익스체인지를 즉시 선언하는 가장 쉬운 방법
이 경우에 이 메서드에 대한 익명 큐를 생성하고 leaning-spring-boot 익스체인지에 바인딩한다
- 이 메서드는 라우팅 키는 comment.new 이며, 이는 learning-spring-boot 익스체인지에 게시된 메시지가 이 메서드를 호출하게됨
- @RabbitListener 메서드는 스프링 AMQP Message 스프링 메시징 Message 다양한 메시지 헤더, 평범하고 오래된 자바 객체를 받을수 이씅ㅁ
- 메서드 자체는 commentWirterRepository 를 호출해서 실제로 데이터 저장소에 코멘트 저장


### AMQP 의 구성요소와 라우팅 알고리즘
- Excahnge: Publisher(Producer)로 부터 수신한 메시지를 큐에 분배하는 라우터 역할
- Queue 메시지를 메모리나 디스크에 저장했다가 Consumer 메세지를 전달하는 역할
- Binding : Exchange와 Queue 관계를 정의한것

## Exchange Type
- Exchange Type 은 메시지를 어떤 방법으로 라우팅할지 결정하는 일종의 알고리즘
- Direct Exchange : Excahnge에 바인딩된 Queue 중에 메시지의 라우팅 키와 매핑되어이 있는 Queue 로 메시지를 전달한다
  1:1 관계로 Unicate 방식에 접합하며, 주로 라운드로빈 방식으로 Workers 간 Task 를 분리에 사용된다
- Fanout Exchange : 메시지의 라우팅 키를 무시하고 Exchange 에 바인딩된 모든 Queue 에 메시지를 전달한다
  1:N 관계로 메시지를 브로드캐스트하는 용도로 사용된다
- Topic Exchange : Exchange 에 바인딩 된 Queue 중에서 메시지의 라우팅 키가 패턴에 맞는 Queue 에서 모두 메세지를 전달한다
  Multicast 방식에 적합하다.
- Headers Exchange: 라우팅 키 대신에 메시지 헤더에 여러 속성들을 더해 속성들이 매칭되는 큐에 메시지를 전달한다.

## RabbitMQ
RabbitMQ는 AMQP 구현한 오픈소스 메시지 소프트웨어 Publisher(Producer)로 부터 메시지를 받아 Consumer 에게 라우트하는 것이 주된 역할

### RabbitMQ Exchange Type
     이름               RabbitMQ 이름
- Direct Exchange : (Empty String) and amq.direct
- Fanout Exchange :  amq.fanout
- Topic Exchange  :  amq.topic
- Header Exchange :  amq.match(and amq.headers in RabbitMQ)


## local 에서 실행하는법
~~~
cd /usr/local/sbin
./rabbitmq-server
~~~

http://localhost:15672로 접근하고 username / password 로 초기값인 guest/guest 로 로그인하면된다