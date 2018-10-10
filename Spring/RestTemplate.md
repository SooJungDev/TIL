## RestTemplate 이란?
 - RestTemplate은 스프링 MVC 라이브러리에 포함된 클래스로 스프링 3.2이상이면 사용 할 수 있음
 - RestTemplate은 다양한 메시지 컨버터를 이미 내장 하고있어서 JSON 응답 또는 Map 또는 모델클래스로 간편하게 변환해서 사용 할수있음
 - RestTemaplate은 Apache htttpClient에 대한 의존성을 가지고 있음 
 
 ## RestTemplate 의존성 추가
  maven에 추가해준다 
  ~~~ xml
      <!-- RestTemplate 관련 httpClient -->
          <dependency>
              <groupId>org.apache.httpcomponents</groupId>
              <artifactId>httpclient</artifactId>
              <version>4.5.6</version>
          </dependency>
  ~~~
  
  ## RestTemplate Bean 설정
  ~~~ java
  @Configuration
  public class RestTemplateConfiguration {
  
      @Bean
      public RestTemplate restTemplate(){
          HttpComponentsClientHttpRequestFactory factory = new HttpComponentsClientHttpRequestFactory();
          factory.setConnectionRequestTimeout(5000); //커넥션 타임아웃 설정
          factory.setReadTimeout(5000);//소켓 read 타임아웃설정
  
          CloseableHttpClient httpClient= HttpClientBuilder.create().build();
          //Http request, response에 기본이되는 클래스 제공, 클라이언트 객체 생성
  
          factory.setHttpClient(httpClient);
  
          RestTemplate restTemplate =new RestTemplate(factory);
          restTemplate.setRequestFactory(new BufferingClientHttpRequestFactory(new SimpleClientHttpRequestFactory()));
          restTemplate.getMessageConverters().add(new MappingJackson2HttpMessageConverter());
          restTemplate.setInterceptors(Collections.singletonList(new RequestResponseLoggingInterceptor()));
  
          return  restTemplate;
  
      }
  }
  ~~~
  
  ## HTTP 메서드별 RestTemplate 메서드 명세
  
  - getForObject : get 방식, 반환타입은 객체
  - getForEntity: get 방식, 반환타입은 HttpResponseEntity
  - postForObject: post 방식, 반환타입은 객체
  - postForEntity: post 방식, 반환타입은 HttpResponseEntity
  - delete: delete 방식, 반환타입은 없음
  - put: put방식, 반환타입 없음
  - exchange: 사용자 지정, 반환타입 사용자 지정  Http Header 를 수정할 수 있고 결과를 Http ResponseEntity로 반환 받을수있음
  
  - example
  ~~~ java
  
//wrapping stringified request-body and HTTP request-headers into HTTP entity and passing it in exchange() method...    
    HttpEntity<String> entity = new HttpEntity<>(requestBody, requestHeaders); 

    restTemplate.exchange("http://localhost:8080/context-path/resource", HttpMethod.POST, entity, String.class);
  ~~~
  
  
  ## 참고사이트
  - https://docs.spring.io/spring/docs/5.1.0.RC1/spring-framework-reference/web.html#webmvc-client
  - https://docs.spring.io/spring/docs/5.1.0.RC1/spring-framework-reference/integration.html#rest-client-access
  - RestTemplate 잘 정리되어있음 http://hoonmaro.tistory.com/46
  - http://sjh836.tistory.com/141
  - http://blog.saltfactory.net/using-resttemplate-in-spring/
  