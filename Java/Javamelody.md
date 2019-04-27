## Javamelody
- JavaEE 어플리케이션을 모니터링 할 수 있는 오픈소스

JavaMelody는 주로 요청 통계 및 진화 차트를 기반으로합니다.

QA 및 프로덕션에서 응용 프로그램을 개선하고 다음 작업을 수행 할 수 있습니다.

- 평균 응답 시간 및 실행 횟수에 대한 정보 제공
- 트렌드가 나쁠 때, 문제가 너무 심각 해지면 의사 결정을 내린다.
- 보다 제한적인 응답 시간을 기준으로 최적화
- 응답 시간의 근본 원인 찾기
- 최적화 후 실제 개선 사항을 확인하십시오.

여기에는 다음 지표의 시간 경과에 따른 진화를 보여주는 요약 차트가 포함됩니다.

- http 요청, SQL 요청, jsf 작업, struts 작업, JSP 페이지 또는 비즈니스 façade 메소드 (EJB3, Spring 또는 Guice 인 경우)의 실행 횟수, 평균 실행 시간 및 오류 백분율
- 자바 메모리
- Java CPU
- 사용자 세션 수
- jdbc 연결 수

pom.xml 에서 추가
- [mvnrepository javamelody](https://mvnrepository.com/artifact/net.bull.javamelody/javamelody-core/1.77.0)
~~~
<!-- https://mvnrepository.com/artifact/net.bull.javamelody/javamelody-core -->
<dependency>
    <groupId>net.bull.javamelody</groupId>
    <artifactId>javamelody-core</artifactId>
    <version>1.77.0</version>
</dependency>
~~~

web.xml에 추가
~~~
<filter>
    <filter-name>javamelody</filter-name>
    <filter-class>net.bull.javamelody.MonitoringFilter</filter-class>
    <async-supported>true</async-supported>
    <init-param>
        <param-name>storage-directory</param-name>
        <param-value>${javamelody.storage.location}</param-value>
    </init-param>		
</filter>
<filter-mapping>
    <filter-name>javamelody</filter-name>
    <url-pattern>/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>ASYNC</dispatcher>
</filter-mapping>
<listener>
    <listener-class>net.bull.javamelody.SessionListener</listener-class>
</listener>
~~~

- 설정해 준뒤 http://서버주소/monitoring 치면 접속가능

## 참고사이트
- [javamelody github](https://github.com/javamelody/javamelody/wiki)
- [Javamelody 간단 사용](https://medium.com/@dongchimi/javamelody-%EA%B0%84%EB%8B%A8-%EC%82%AC%EC%9A%A9-91c3dd902b7f)
- [Tomcat Web Application 모니터링 툴 – javamelody 설정 방법](https://everydayminder.wordpress.com/2013/08/01/tomcat-web-application-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%ED%88%B4-javamelody-%EC%84%A4%EC%A0%95-%EB%B0%A9%EB%B2%95/)
