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
~~~java
@Override
protected void configure(HttpSecurity http) throws Exception{
    http.anonymous()
         .and()
         .formLogin()
         .and()
         .authorizeRequests()
         .mvcMatchers(HttpMethod.GET, "/api/**").autheniticated()
         .anyRequest()
         .authenticated();
}
~~~
- 익명 사용자 사용 활성화
- 폼 인증 방식 활성화
    - 스프링 시큐리티가 기본 로그인 페이지 제공
- 요청에 인증 적용
    - /api 이하 모든 GET 요청에 인즈이 필요함.
    - permitAll()을 사용하여 인증이 필요없이 익명으로 접근이 가능케 할 수 있음
    - 그밖에 모은 요청도 인증이 필요함
    
    
## 스프링 시큐리티 OAuth 2 설정: 인증 서버 설정
- 의존성 추가
~~~
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
     <version>${spring-security.version}</version>
     <scope>test</scope> 
</dependency>
~~~

토큰 발행 테스트
- User
- Client
- POST /oauth/token
    - 요청 매개변수 (MultiValueMap<String,String>)
        - grant_type:password
        - username
        - password
    - 응답에서 access_token 나오는지 확인
    
Grant Type:Password
- Grant Type: 토큰받아오는 방법
- 서비스 오너가 만든 클라이언트에서 사용하는 Grant Type
- [What is the OAuth 2.0 Password Grant Type?](https://developer.okta.com/blog/2018/06/29/what-is-the-oauth2-password-grant)

AuthorizationServer 설정
- @EnableAuthorizationServer
- extends AuthorizationServerConfigurerAdapter
- configure(AuthorizationServerSecurityConfigurer security)
    - PasswordEncode 설정
- configure(ClientDetailsServiceConfigurer clients)
    - 클라이언트 설정
    - grantTypes
        - password
        - refresh_token
    - scopes
    - secret / name
    - accessTokenValiditySeconds
    - refreshTokenValiditySeconds
- AuthorizationServerEndpointsConfigurer
    - tokenStore
    - autheniticationManager
    - userDetailsService
    


## 스프링 시큐리티 OAuth 2 설정: 인증 서버 설정
테스트 수정
- GET 을 제외하고 모두 액세스 토큰을 가지고 요청하도록 테스트 수정

ResourceServer 설정
- @EnableResourceServer
- extends ResourceServerConfigurerAdapter
- configure(ResourceServerSecurityConfigurer resources)
    - 리소스 ID
- configure(HttpSecurity http)
    - anonymous
    - GET /api/** : permit all
    - POST /api/** : authenticated
    - PUT /api/** : authenticated
    - 에러 처리

## 문자열을 외부 설정으로 빼내기
기본 유저 만들기
- ApplicationRunner
    - Admin
    - User
 
외부설정으로 기본 유저와 클라이언트 정보 빼내기
- @ConfigurationProperties   

## 이벤트 API 점검
토큰 발급받기
- POST /oauth/token
- BASIC authentication 헤더
    - client id(myApp)+ client secret(pass)
- 요청 본문 폼
    - username: admin@email.com
    - password: admin
    - grant_type: password
    

토큰 갱신하기     
- POST /oauth/token
- BASIC authentication 헤더
    - client id(myApp)+ client secret(pass) 
- 요청 본문 폼
    - token: 처음 받았던 refresh 토큰
    - grant_type: refresh_token

이벤트 목록 조회
- 로그인 했을때
    - 이벤트 생성 링크 제공

이벤트 조회
- 로그인 했을때
    - 이벤트 Manager 인 경우에는 이벤트 수정 링크 제공    

## 스프링 시큐리티 현재 사용자
SecurityContext
- 자바 ThreadLocal 기반 구현으로 인증 정보를 담고 있음
- 인증정보 꺼내는 방법 밑에와 같음
~~~java
Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
User principal = (User) authentication.getPrincipal();
~~~

@AuthenticationPrincipal spring.security.User user
- 인증 안한 경우에 null
- 인증 한 경우에는 username과 authorities 참조 가능


spring.security.User를 상속받는 클래스를 구현하면
- 도메인 User를 받을 수 있다.
- @AuthenticationPrincipal me.crystal.user.UserAdapter
- Adapter.getUser().getId

SpEL을 사용하면
- @AuthenticationPrincipal(expression="account") me.crystal.user.Account

~~~
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@AuthenticationPrincipal(expression="account")
public @interface CurrentUser{

}
~~~
커스텀 애노테이션을 만들려면
- @CurrentUser Account account
- 인증안하고 접근할 경우???

expression ="#this == 'anonymousUser' ? null : account"
- 현재 인증 정보가 anonymousUser 인 경우에는 null을 보내고 아니면 "account"를 꺼내준다

조회 API 개선
- 현재 조회하는 사용자가 owner 인 경우에 update 링크 추가 (HATEOAS)

수정 API 개선 현재 사용자가 이벤트 owner 가 아닌 경우에 403 에러 발생


## Events API 개선: 출력값 제한하기
생성 API  개선
- Event owner 설정
- 응답에서 owner의 id만 보내줄것
- JsonSerializer<User> 구현
- @JsonSerialize(using) 설정


## 깨진 테스트 살펴보기
EventControllerTests.updateEvent()
- 깨지는 이유: NullPointerException
- 해결방법: 코드 수정

EventControllerTests.getEvent()
- 깨지는 이유: NullPointerException
- 해결방법: 코드수정

ApplicationTests
- 깨지는 이유: 테스트에서 H2가 아닌 Postgre SQL 사용하려 하지만, PostgreSQL이 동작중이지 않음
- 해결방법: 해당 테스트에서 H2 사용하도록 test 프로파일 설정
