## Account 도메인 추가
OAuth2로 인증 하려면 일단 Account 부터
- id
- email
- password
- roles

AccountRoles
- ADMIN,USER

JPA 맵핑
- @Table("Users")

JPA enumeration collection mapping

~~~java
@ElementCollection(fetch = FetchType.EAGER)
@Enumerated(EnumType.STRING)
private Set<AccountRole> roles;
~~~

Event에 owner 추가
~~~java
@ManyToOne  
Account manager;
~~~

## 스프링 시큐리티
- 웹 시큐리티(Filter 기반 시큐리티)
- 메소드 시큐리티
- 이 둘다 Security Interceptor를 사용합니다.
    - 리소스에 접근을 허용 할것이냐 말것이냐 결정하는 로직이들어있음.
 
의존성추가
~~~
<dependency>
    <groupId>org.springframework.security.oauth.boot</groupId>
    <artifactId>spring-security-oauth2-autoconfigure</artifactId> 
    <version>2.1.0.RELEASE</version>
</dependency>
~~~

테스트 다 깨짐(401 Uauthorized)
- 깨지는 이유는 스프링부트가 제공하는 스프링 시큐리티 기본 설정때문.

## 예외 테스트
1.@Test(expected)
- 예외타입만 확인 가능

2. try-catch
- 예외 타입과 메시지 확인가능
- 하지만 코드가 다소 복잡

3. @Rule ExpectedException
- 코드는 간결하면서 예외 타입과 메시지 모두 확인 가능 

## 스프링 시큐리티 기본 설정
시큐리티 필터를 적용하지 않음
- /docs/index.html

로그인 없이 접근가능
- GET /api/events
- GET /api/events/{id}

로그인 해야 접근가능
- 나머지다
- POST /api/events
- PUT /api/events/{id}
- ...

스프링 시큐리티 OAuth 2.0
- AuthorizationServer: OAuth2 토큰 발행 (/oauth/token) 및 토큰 인증(/oauth/authorize)
    - Order 0 (리소스 서버보다 우선순위가 높다)
- ResourceSever: 리소스 요청 인증처리(OAuth2 토큰 검사)
    - Order 3(이 값은 현재 고칠 수 없음)

스프링 시큐리티 설정
- @EnableWebSecurity
- @EnableGlobalMethodSecurity
- extends WebSecurityConfigurerAdapter
- PasswordEncoder: PasswordEncoderFactories.createDelegatingPasswordEncoder()
- TokenStore: InMemoryTokenStore
- AutheniticationManagerBean
- configure(AuthenticationManagerBuilder auth)
    - userDetailsService
    - passwordEncoder
    
- configure(HttpSecurity http)
    - /doc/** : permitAll
- configure(WebSecurity web)
    - ignore
        - /docs/**
        - favicon.ico
- PathRequest.toStaticResources() 사용하기

## 스프링 시큐리티 폼 인증 설정
    
## 스프링 시큐리티 OAuth 2 설정: 인증 서버 설정

## 스프링 시큐리티 OAuth 2 설정: 인증 서버 설정

## 문자열을 외부 설정으로 빼내기

## 이벤트 API 점검

## 스프링 시큐리티 현재 사용자

## Events API 개선: 출력값 제한하기

## 깨진 테스트 살펴보기
