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
  - @ComponentScan: 빈을 스캔해서 다 등록한다음에
  - @EnableAutoConfiguration : 정보를 바탕으로 원래 있던 빈들에 기존에 있던걸 반영하고 없으면 빈을 자동으로 만들어짐 
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

## HTTPS와 HTTP2
- 키스토어 만든뒤 설정해줌
- HTTPS 설정후 HTTP 로 접속할수가 없음 커넥터가 하나기 떄문에
- Http2 설정
 - http2.enable
 - 사용하는 서블릿 컨테이너 마다 다름
 - 톰캣8 jdk1.8은 추가적으로 설정해줘야하는게 있으므로 버젼올리는것을 추천
 
 ## 독립적으로 실행 가능한 jar
  - mvn package를 하면 실행 가능한 jar파일 하나가 새성됨
  - 그 jar파일 안에 다들어있음
  - spring-maven-plugin이 해주는 일(패키징) 
  - 스프링 부트 전략
   - 내장 jar : 기본적으로 자바에는 내장 jar 로딩하는 표준적인 방법 x
   - 애플리케이션 클래스와 라이브러리 위치구분함
   - org.springframework.boot.loader.jar.JarFile 을 사용해서 내장 jar읽는다
   - org.springframework.boot.loader.Launchar을 사용해서 실행한다 (내장된 라이브러리 jar파일 모두 읽는다)
   - 단독적인 파일로 실행되는것은 스프링부트의 주요 목적임 

## 스프링부트 원리
- 의존성 관리(spring-boot-starter)
  - maven에 parent, dependencies management
- 자동설정
  - Springboot 빈을 두단계로 나눠서 읽어서 등록
    - @ComponentScan: 빈을 스캔해서 다 등록한다음에
    - @EnableAutoConfiguration : 정보를 바탕으로 jar파일에 들어있는 meta-inf 중에 spring.factories 그안에 들어가있는 autoconfigurationclass 참조해서 자동설정
      - 원래있던 빈들에 기반을함 자동설정파일을 활용해서 재정의 하거나 설정해야될것을 줄여주고있음
  
- 내장 웹서버 (standalone 한 app 만들기위해서 springboot -> 서버아님!! 활용해서 쓰는것뿐)
- 독립적으로 실행 가능한 jar 

 
## 참고사이트
  - [인프런 강의 자동설정 이해](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/%EC%9E%90%EB%8F%99-%EC%84%A4%EC%A0%95-%EC%9D%B4%ED%95%B4/)