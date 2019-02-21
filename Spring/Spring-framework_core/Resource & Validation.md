## Resource 추상화
org.springframework.core.io.Resource

특징
- java.net.URL을 추상화 한것.
- 스프링 내부에서 많이 사용하는 인터페이스

추상화 한이유
- 클래스 패스 기준으로 리소스 읽어오는 기능부재
- ServletContext를 기준으로 상대 경로로 읽어오는 기능부재
- 새로운 핸들러를 등록하여 특별한 URL 접미사를 만들어 사용 할 수 있지만 구현이 복잡하고 편의성 메소드가 부족하다.

[인터페이스 둘러보기](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/io/Resource.html)
- 상속 받은 인터페이스
- 주요메소드
    - getInputStream()
    - exitst()
    - isOpen()
    - getDescription() : 전체 경로 포함한 파일 이름 또는 실제 URL
    
구현체
- UrlResource: [java.net.URL](https://docs.oracle.com/javase/7/docs/api/java/net/URL.html) 참고
    - 기본으로 지원하는 프로토콜 http, https, ftp, jar.
- ClassPathResource: 지원하는 접두어 classpath:
- FileSystemResource
- ServletContextResource: 웹애플리케이션 루트에서 상대 경로로로 리소스를 찾는다.

리소스 읽어오기
- Resource 타입은 location 문자열과 ApplicationContext의 타입에 따라 결정된다.
    - ClassPathXmlApplicationContext -> ClassPathResource
    - FileSystemXmlApplicationContext -> FileSystemResource
    - WebApplicationContext -> ServletContextResource
- ApplicationContext 의 타입에 상관없이 리소스 타입을 강제하려면 java.net.URL 접두어(+classpath:) 중 하나를 사용할 수있다.
    - classpath:me/crystal/config.xml -> ClassPathResource
    - file://some/resource/path/config.xml -> FileSystemResource
    
    
## Validation 추상화
[Validator](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/validation/Validator.html)
       
애플리케이션에서 사용하는 객체 검증용 인터페이스  

특징  
- 어떠한 계층과도 관계가 없다. => 모든계층(웹,서비스.데이터)에서 사용해도 좋다.
- 구현체 중 하나로 , JSR-303(Bean Validation 1.0)과 JSR-349(Bean Validation 1.1)을 지원한다.([LocalValidatorFactoryBean](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/validation/beanvalidation/LocalValidatorFactoryBean.html))
- DataBinder에 들어가 바인딩 할때 같이 사용되기도 한다.

인터페이스
- boolean supports(Class class): 어떤 타입의 객체를 검증할때 사용할 것인지 결정함
- void validate(Object obj, Errors e): 실제 검증 로직을 이안에서 구현
    - 구현할때 ValidationUtils 사용하며 편리함.
    
스프링 부트 2.0.5 이상 버전을 사용 할때
- LocalValidatorFactoryBean 빈으로 자동 등록
- JSR-380(Bean Validation 2.0.1) 구현체로 hibernate-validator 사용
- https://beanvalidation.org/