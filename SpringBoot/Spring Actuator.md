## 스프링 부트 Actuator
의존성 추가
- spring-boot-starter-actuator

애플리케이션의 각종 정보를 확인할 수 있는 Endpoints

- 다양한 Endpoints 제공.
- JMX 또는 HTTP를 통해 접근 가능 함.
- shutdown을 제외한 모든 Endpoint는 기본적으로 활성화 상태.
- 활성화 옵션 조정
    - management.endpoints.enabled-by-default=false
    - management.endpoint.info.enabled=true
    
##  JMX와 HTTP
JConsole 사용하기
- https://docs.oracle.com/javase/tutorial/jmx/mbeans/
- https://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html

VisualVM 사용하기
- https://visualvm.github.io/download.html

HTTP 사용하기
- /actuator
- health와 info를 제외한 대부분의 Endpoint가 기본적으로 비공개 상태
- 공개 옵션 조정
 - management.endpoints.web.exposure.include=*
 - management.endpoints.web.exposure.exclude=env,beans

## 스프링 부트 어드민
https://github.com/codecentric/spring-boot-admin

스프링 부트 Actuator UI 제공

어드민 서버 설정
~~~
<dependency>
  <groupId>de.codecentric</groupId>
  <artifactId>spring-boot-admin-starter-server</artifactId>
  <version>2.0.1</version>
</dependency>

@EnableAdminServer
~~~

클라이언트 설정
~~~
<dependency>
  <groupId>de.codecentric</groupId>
  <artifactId>spring-boot-admin-starter-client</artifactId>
  <version>2.0.1</version>
</dependency>

spring.boot.admin.client.url=http://localhost:8080
management.endpoints.web.exposure.include=*
~~~
