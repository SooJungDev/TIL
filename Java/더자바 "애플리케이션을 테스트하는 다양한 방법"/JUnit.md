# 1부 JUnit
## JUnit5: 소개
자바 개발자가 가장 많이 사용하는 테스팅 프레임 워크
- https://www.jetbrains.com/lp/devecosystem-2019/java/
    - 단위 테스트를 작성하는 자바 개발자 93% JUnit 을 사용함.
- 자바 8이상을 필요로함
- 대체제: TestNG,Spock...

- Platform: 테스트를 실행해주는 런처 제공. TestEngine API 제공
- Jupiter: TestEngine API 구현체로 JUnit5를 제공
- Vintage: JUnit4와 3을 지원하는 TestEngine 구현체

참고:
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

## JUnit5: 시작하기
스프링 부트 프로젝트 만들기
- 2.2+ 버전의 스프링 부트 프로젝트를 만든다면 기본으로 JUnit5 의존성 추가됨

스프링부트 프로젝트 사용하지 않는다면?
~~~
<dependency> 
     <groupId>org.junit.jupiter</groupId>
     <artifactId>junit-jupiter-engine</artifactId>
     <version>5.5.2</version>
     <scope>test</scope>
</dependency>
~~~

기본 애노테이션
- @Test
- @BeforeAll / @AfterAll
- @BeforeEach / @AfterEach
- @Disabled

## JUnit5: 테스트 이름 표시하기
@DisplayNameGeneration
- Method 와 Class 레퍼런스를 사용해서 테스트 이름을 표기하는 방법 설정
- 기본 구현체로 ReplaceUnderscores 제공

@DisplayName
- 어떤 테스트인지 테스트 이름을 보다 쉽게 표현 할 수 있는 방법을 제공하는 애노테이션
- @DisplayNameGeneration 보다 우선 순위가 높다

참고: 
- [Display Names](https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names)

