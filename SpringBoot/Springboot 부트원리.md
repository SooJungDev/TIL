## 의존성 관리 응용
-  스프링 부트가 관리해주지 않는 의존성이면 
 - maven 관리 해 주지 않는것은 maven dependency에 version을 명시해주는 것이 제일 좋음

## 자동 설정 이해 
~~~ java
//@SpringBootApplication
@SpringBootConfiguration
@ComponentScan
@EnableAutoConfiguration
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);

    }
}
~~~
 - 스프링부트 사실상으로 빈을 두단계로 나눠서 읽힘
     - @ComponentScan: 빈을 다 등록한 다음에
     - @EnableAutoConfiguration : 빈을 자동으로 만들어짐 
 - @ComponentScan : 자신이 달려있는 하위에있는 패키지에 있는것들을 다읽어드림 다른패키지에 존재할 경우 읽어드리지 못함(패키지 주의해야함)
  - @Component 로 선언된 애들을 읽음
  - @Configuration @Repository @Service @Controller @RestController
 - @EnableAutoConfiguration
  - spring.factories (자동설정 키 값들이 모여져있)
   - org.springframework.boot.autoconfigure.EnableAutoConfiguration
  - @Configuration
  - @ConditionalOnXxxYyyZzz : 조건에 따라 어떠한 빈을 등록하기도(없을때만)하고 안하기도 함
  
## 자동 설정 구현
- Xxx-Spring-Boot-Autoconfigure 모듈: 자동 설정
- Xxx-Spring-Boot-Starter 모듈: 필요한 의존성 정의
- 그냥 하나로 만들고 싶을 때는?
    - Xxx-Spring-Boot-Starter
       
구현방법
1.의존성 추가
~~~
<dependencies>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-autoconfigure</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-autoconfigure-processor</artifactId>
      <optional>true</optional>
  </dependency>
</dependencies>

<dependencyManagement>
  <dependencies>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-dependencies</artifactId>
          <version>2.0.3.RELEASE</version>
          <type>pom</type>
          <scope>import</scope>
      </dependency>
  </dependencies>
</dependencyManagement>
~~~
2.@Configuration 파일작성
3.src/main/resource/META-INF에 spring.factories 파일 만들기
4.spring.factories 안에 자동 설정 파일 추가
~~~
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
설정파일 경로 추가 ex)  me.crystal.HolomanConfiguration
~~~
5.maven install 해줌

- 스프링부트 빈을 등록할때 두군데에서 하기 때문에 덮어 써지는 문제가 생길 수 있음
  - @ComponentScan: 빈을 다 등록한다음에
  - @EnableAutoConfiguration : 빈을 자동으로 만들어짐 
- 덮어 쓰기 방지하기 
  - @ConditionalOnMissingBean
- 빈 재정의 수고 덜기
 - @ConfigurationProperties("holoman")
 - @EnableConfigurationProperties(HolomanProperties)
 - 프로퍼티 키값 자동 완성
 
 ~~~
 dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
 </dependency>
 ~~~
 
 ## 내장 웹서버 이해 
 - 톰캣 객체 설정
 - 포트 설정
 - 톰캣에 컨텍스트 추가
 - 서블릿 만들기
 - 톰캣에 서블릿 추가
 - 컨텍스트에 서블릿 맵핑
 - 톰캣 실행 및 대기
 
 => 이모든 과정을 보다 상세히 유연하고 설정하고 실행해주는게 바로 스프링 부트 자동설정
 
- ServletWebServerFactoryAutoConfiguration (서블릿 웹 서버 생성)
  - TomcatServletWebServerFactoryCustomizer (서버 커스터마이징)
- DispatcherServletAutoConfiguration : 서블릿 만들고 등록

## 내장 웹서버 응용 (컨테이너와 포트)
- 다른 서블릿 컨테이너로 변경
- 웹 서버 사용 하지 않기
- 포트
server.port
랜덤 포트
ApplicationListner<ServletWebServerInitializedEvent>

## 참고사이트
  - [인프런 강의 자동설정 이해](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/%EC%9E%90%EB%8F%99-%EC%84%A4%EC%A0%95-%EC%9D%B4%ED%95%B4/)