## 스프링부트 시큐리티
- 웹시큐리티
- 메소드 시큐리티
- 다양한 인증방법 지원
 - LDAP,폼인증, Basic 인증, OAuth, 등등
 
## 스프링부트 시큐리티 자동설정
- SecurityAutoConfiguration
- UserDetailsServiceAutoConfiguration
- spring-boot-starter-security
  - 스프링 시큐리티5.* 의존성 추가
- 모든 요청에 인증이 필요
- 기본 사용자 생성
 - Username: user
 - Password: 애플리케이션을 실행할때마다 랜덤값 생성
 - spring.security.user.name
 - spring.security.user.password
- 인증 관련 각종 이벤트 발생
 - DefaultAuthenticationEventPublisher 빈 등록
 - 다양한 인증 에러 핸들러 등록 가능


## 웹시큐리티 설정
~~~java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .antMatchers("/","/hello").permitAll() // 두가지에 관해서는 권한을 줄거고
            .anyRequest().authenticated() // 나머지 요청은 모두 인증이 필요하다
            .and()
        .formLogin()
            .and()
        .httpBasic();
    }

    @Bean
    public PasswordEncoder passwordEncoder(){
        return PasswordEncoderFactories.createDelegatingPasswordEncoder();
    }
}

~~~

- UserDetailsService 구현
~~~java
Service
public class AccountService implements UserDetailsService {

    @Autowired
    private AccountRepository accountRepository;

    @Autowired
    private PasswordEncoder passwordEncoder;

    public Account createAccount(String username, String password) {
        Account account = new Account();
        account.setUsername(username);
        account.setPassword(passwordEncoder.encode(password));
        return accountRepository.save(account);
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Optional<Account> byUsername=accountRepository.findByUsername(username);
        Account account=byUsername.orElseThrow(() -> new UsernameNotFoundException(username));
        return new User(account.getUsername(), account.getPassword(), authorities());
    }

    private Collection<? extends GrantedAuthority> authorities() {
        return Arrays.asList(new SimpleGrantedAuthority("ROLE_USER"));
    }
}


~~~

- PasswordEncoder 설정 및 사용


## 참고사이트
  - [백기선님 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/starter-security/)
  - [spring security 레퍼런스](https://docs.spring.io/spring-security/site/docs/current/reference/html/)