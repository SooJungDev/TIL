# 1부 JUnit
## JUnit5: 소개
자바 개발자가 가장 많이 사용하는 테스팅 프레임 워크
- https://www.jetbrains.com/lp/devecosystem-2019/java/
    - 단위 테스트를 작성하는 자바 개발자 93% JUnit 을 사용함.
- 자바 8이상을 필요로함
- 대체제: TestNG,Spock...
- JUnit5 나온지 꽤됨 스프링부트 2.2+기본 JUnit5를 올림

세부모듈
- Platform: 테스트를 실행해주는 런처 제공. TestEngine API 제공
- Jupiter: TestEngine API 구현체로 JUnit5를 제공 (주로 이쪽을 학습하게 됨)
- Vintage: JUnit4와 3을 지원하는 TestEngine 구현체

참고:
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

## JUnit5 시작하기
스프링 부트 프로젝트 만들기
- 2.2+ 버전의 스프링 부트 프로젝트를 만든다면 기본으로 JUnit5 의존성 추가됨

스프링부트 프로젝트 사용하지 않는다면?
아래 의존성만 추가해주면 됨
~~~
<dependency> 
     <groupId>org.junit.jupiter</groupId>
     <artifactId>junit-jupiter-engine</artifactId>
     <version>5.5.2</version>
     <scope>test</scope>
</dependency>
~~~

메소드가 퍼블릭이 아니더라도 실행된다

~~~java
class StudyTest {

    @Test
    void create(){
        Study study = new Study();
        assertNotNull(study);
        System.out.println("create");
    }

    @Test
    void create1(){
        System.out.println("create1");
    }

    @BeforeAll
    static void beforeAll(){
        System.out.println("before all");
    }

    @AfterAll
    static void afterAll(){
        System.out.println("after all");
    }

    @BeforeEach
    void beforeEach(){
        System.out.println("before each");
    }

    @AfterEach
    void afterEach(){
        System.out.println("After each");
    }
}
~~~

실행결과
~~~
before all

before each
create
After each

before each
create1
After each

after all
~~~

기본 애노테이션
- @Test
- @BeforeAll / @AfterAll
- @BeforeEach / @AfterEach
- @Disabled : 테스트가 꺠졌을떄 무시하고 실행 깨졌을떄 제일 베스트는 고치는게 좋음 근데 부득이하게 실행할 경우에 해당 어노테이션 사용

## JUnit5: 테스트 이름 표시하기
~~~java
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class)
class StudyTest {

    @Test
    @DisplayName("스터디 만들기 \uD83D\uDE2D")
    void create_new_study(){
        Study study = new Study();
        assertNotNull(study);
        System.out.println("create");
    }

    @Test
    @DisplayName("스터디 만들기 \uD83D\uDC4D\uD83D\uDE2D")
    void create_new_study_again(){
        System.out.println("create1");
    }

~~~
@DisplayNameGeneration
- Method 와 Class 레퍼런스를 사용해서 테스트 이름을 표기하는 방법 설정
- 기본 구현체로 ReplaceUnderscores 제공

@DisplayName
- 어떤 테스트인지 테스트 이름을 보다 쉽게 표현 할 수 있는 방법을 제공하는 애노테이션
- @DisplayNameGeneration 보다 우선 순위가 높다

참고: 
- [Display Names](https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names)

## JUnit5: Assertion
org.junit.jupiter.api.Assertions.*

~~~java
IllegalArgumentException exception = assertThrows(IllegalArgumentException.class,
                                                  () -> new Study(-10));
String message = exception.getMessage();
assertEquals("limit 은 0보다 커야 한다.", message);
Study study = new Study(-10);
assertAll(
        () -> assertNotNull(study),
        () -> assertEquals(StudyStatus.DRAFT, study.getStatus(),
                           () -> "스터디를 처음 만들면" + StudyStatus.DRAFT + "상태다"),
        () -> assertTrue(study.getLimit() > 0, "스터디 최대 참석 가능 인원은 0보다 커야 한다.")
);
~~~

- 실제값이 기대한 값과 같은지 확인 : assertEquals(expected, actual)
- 값이 null이 아닌지 확인 : assertNotNull(actual)
- 다음 조건이 참(true)인지 확인 : assertTrue(boolean)
- 모든 확인 구문 확인 : assertAll(executables...)
- 예외 발생 확인 : assertThrows(expectedType, executable)
- 특정 시간안에 실행이 완료되는지 확인 : assertTimeout(duration, executable)

마지막 매개변수로 Supplier<String> 타입의 인스턴스를 람다 형태로 제공 할 수 있다.
~~~java
 assertEquals(StudyStatus.DRAFT, study.getStatus(), () -> "스터디를 처음 만들면"+ StudyStatus.DRAFT +"상태다");
~~~
- **복잡한 메세지 생성 해야 하는 경우 사용하면 실패한 경우에만 해당 메시지를 만들게 할 수 있다.**

- assertTimeoutPreemptively 주의해서 사용해야됨
    - ThreadLocal 때문에
~~~java
       assertTimeoutPreemptively(Duration.ofMillis(100), () -> {
            new Study(10);
            Thread.sleep(300);
        });
        // TODO ThreadLocal
~~~

 [AssertJ](https://joel-costigliola.github.io/assertj/),[Hamcrest](https://hamcrest.org/JavaHamcrest/),[Truth](https://truth.dev/)
등의 라이브러리를 사용 할 수 도있다.
- AssertJ, Hamcrest 정도는 알아두면 좋음

## JUnit5: 조건에 따라 테스트 실행하기
특정한 조건을 만족하는 경우에 테스트를 실행 하는 방법
~~~java
    @Test
    @DisplayName("스터디 만들기 \uD83D\uDE2D")
    void create_new_study() {
        String test_env = System.getenv("TEST_ENV");

        assumingThat("LOCAL".equalsIgnoreCase(test_env), ()->{
            System.out.println("local");
            Study actual = new Study(100);
            assertThat(actual.getLimit()).isGreaterThan(0);
        });


        assumingThat("soojung".equalsIgnoreCase(test_env), ()->{
            System.out.println("soojung");
            Study actual = new Study(10);
            assertThat(actual.getLimit()).isGreaterThan(0);
        });

    }
~~~
org.junit.jupiter.api.Assumptions.*
- assumeTrue(조건)
- assumingThat(조건,테스트)


~~~java
    @Test
    @DisplayName("스터디 만들기 \uD83D\uDE2D")
    @EnabledOnOs({ OS.MAC, OS.LINUX})
    void create_new_study() {
        String test_env = System.getenv("TEST_ENV");
        System.out.println("local");
        Study actual = new Study(100);
        assertThat(actual.getLimit()).isGreaterThan(0);
    }

    @Test
    @DisplayName("스터디 만들기 \uD83D\uDC4D\uD83D\uDE2D")
    @DisabledOnOs({ OS.MAC})
    void create_new_study_again() {
        System.out.println("create1");
    }

~~~
@Enabled ___ 와 @Disabled___
- OnOS
- OnJre
- IfSystemProperty
- IfEnvironmentVariable
- If (현재 지원하지않음)

## JUnit5: 태깅과 필터링
테스트 그룹을 만들고 원하는 테스트 그룹만 테스트를 실행 할 수 있는 기능
@Tag
- 테스트 메소드에 태그를 추가 할 수 있다.
- 하나의 테스트 메소드에 여러 태그를 사용 할 수 있다.

인텔리제이에서 특정 태그로 테스트 필터링 하는 방법

메이븐에서 테스트 필터링 하는방법
~~~
<plugin> 
    <artifactId>maven-surefire-plugin</artifactId>
     <configuration>
        <groups>fast | slow</groups>
     </configuration>
</plugin>
~~~
- [Introduction to Build Profiles](https://maven.apache.org/guides/introduction/introduction-to-profiles.html)
- [Tag Expressions](https://junit.org/junit5/docs/current/user-guide/#running-tests-tag-expressions)

## JUnit5:커스텀 태그
JUnit5 애노테이션을 조합하여 커스텀 태그를 만들 수 있다.

FastTest.java
~~~java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Test
@Tag("fast")
public @interface FastTest {
}

~~~

~~~java
@FastTest
@DisplayName("스터디 만들기 fast")
void create_new_study(){


@SlowTest
@DisplayName("스터디 만들기 slow")
void create_new_study_again(){
~~~

## JUnit5: 테스트 반복하기 1부
~~~java
    @DisplayName("스터디 만들기")
    @RepeatedTest(value = 10, name ="{displayName}, {currentRepetition}/{totalRepetitions}" )
    void repeatTest(RepetitionInfo repetitionInfo){
        System.out.println("test"+repetitionInfo.getCurrentRepetition()+"/"+repetitionInfo.getTotalRepetitions());
    }

    @DisplayName("스터디 만들기")
    @ParameterizedTest(name = "{index} {displayName} message={0}")
    @ValueSource(strings = {"날씨가","많이","추워지고","있네요."})
    void parameterizedTest(String message){
        System.out.println(message);
    }

~~~
@RepeatedTest
- 반복 횟수와 반복 테스트 이름을 설정 할 수 있다.
    - {displayName}
    - {currentRepetition}
    - {totalRepetitions}
- RepetitionInfo 타입의 인자를 받을 수 있다.

@ParameterizedTest
- 테스트에 여러 다른 매개변수를 대입해가며 반복 실행한다.
    - {displayName}
    - {index}
    - {arguments}
    - {0},{1},...
    
## JUnit5: 테스트 반복하기 2부
인자 값들의 소스
- @ValueSource
- @NullSource, @EmptySource, @NullAndEmptySource
- @EnumSource
- @MethodSource
- @CvsSource
- @CvsFileSource
- @ArgumentSource

인자값 타입 변환
- 암묵적인 타입 변환
 - [레퍼런스 참고](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests-argument-conversion-implicit)
- 명시적 타입 변환
    - SimpleArgumentConverter 상속 받은 구현체 제공
    - @ConvertWith
    
인자값 조합
- ArgumentsAccessor
- 커스텀 Accessor
    - ArgumentsAggregator 인터페이스 구현
    - @AggregateWith
    
~~~java
    @DisplayName("스터디 만들기")
    @ParameterizedTest(name = "{index} {displayName}")
    @CsvSource({"10, '자바 스터디'","20, 스프링"})
    void parameterizedTest(@AggregateWith(StudyAggregator.class) Study study) {
        System.out.println(study);
    }

    static class StudyAggregator implements ArgumentsAggregator {

        @Override
        public Object aggregateArguments(ArgumentsAccessor argumentsAccessor, ParameterContext parameterContext)
                throws ArgumentsAggregationException {
            return new Study(argumentsAccessor.getInteger(0), argumentsAccessor.getString(1));
        }
    }

    static class StudyConverter extends SimpleArgumentConverter{

        @Override
        protected Object convert(Object source, Class<?> targetType) throws ArgumentConversionException {
            assertEquals(Study.class,targetType, "Can only convert to Study");
            return new Study(Integer.parseInt(source.toString()));
        }
    }
~~~

참고
- [Parameterized Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests)

## JUnit5 : 테스트 인스턴스
JUnit 은 테스트 메소드마다 테스트 인스턴스룰 새로만든다
- 이것이 기본 전략
- 테스트 메소드를 독립적으로 실행하여 예상치 못한 부작용을 방지하기 위함이다.
- 이 전략을 JUnit5 에서 변경 할 수 있다

@TestInstance(Lifecycle.PER_CLASS)
- 테스트 클래스당 인스턴스를 하나만 만들어 사용한다
- 경우에 따라 테스트 간에 공유하는 모든 상태를 @BeforeEach 또는 @AfterEach에서 초기화 할 필요가 있다
- @BeforeAll 과 AfterAll 을 인스턴스 메소드 또는 인터페이스에 정의한 default 메소드로 정의할 수도 있다.

## JUnit5: 테스트 순서
실행할 테스트 메소드 특정한 순서에 의해 실행되지만 어떻게 그 순서를 정하는지 의도적으로 분명히 하지 않는다.
(테스트 인스턴스를 테스트 마다 새로 만드는 것과 같은 이유)

경우에 따라, 특정 순서대로 테스트를 실행하고 싶을 때도 있다.
그 경우에는 테스트 메소드를 원하는 순서에 따라 실행하도록 @Testinstance(Lifecycle.PER_CLASS)와 함께
@TestMethodOrder 를 사용 할 수 있다.
- MethodOrderer 구현체를 설정한다.
- 기본 구현체
    - Alphanumeric
    - OrderAnnotation
    - Random
    
## JUnit5: junit-platform.properties
JUnit 설정 파일로, 클래스패스 루트 (src/test/resources/)에 넣어두면 적용된다.

테스트 인스턴스 라이프사이클 설정
junit.jupiter.testinstance.lifecycle.default = per_class

확장팩 자동 감지 기능 
junit,jupiter.extensions.autodetection.enabled = true

@Disabled 무시하고 실행하기
junit.jupiter.conditions.deactivate = org.junit.*DisabledCondition

테스트 이름 표기 전략 설정
junit.jupiter.displayname.generator.default = \ 
org.junit.jupiter.api.DisplayNameGenerator$ReplaceUnderscores

## JUnit5: 확장 모델
JUnit4의 확장모델은 @RunWith(Runner), TestRule, MethodRule.
JUnit5의 확장모델은 단 하나 , Extension

확장팩 등록 방법
- 선언적인 등록 @ExtendWith
- 프로그래밍 등록 @RegisterExtension
- 자동 등록 자바 ServiceLoader 이용

확장팩 만드는 방법
- 테스트 실행 조건
- 테스트 인스턴스 팩토리
- 테스트 인스턴스 후 - 처리ㅈ기
- 테스트 매개변수 리졸버
- 테스트 라이프사이클 콜백
- 예외처리
- ...

참고
- [extensions](https://junit.org/junit5/docs/current/user-guide/#extensions)

## JUnit5: JUnit4 마이그레이션
junit-vintage-engine 을 의존성으로 추가하면, JUnit5의 junit-platform으로 JUnit3과 4로 작성된 테스트를 실행 할 수 있다.
- @Rule은 기본적으로 지원하지 않지만, junit-jupiter-migrationsupport 모듈이 제공하는
@EnableRuleMigrationSupport 를 사용하면 다음 타입의 Rule을 지원한다.
- ExternalResource
- Verifier
- ExpectedException

JUnit4
- @Category(Class)
- @RunWith, @Rule, @ClassRule
- @Ignore
- @Before, @After, @BeforeClass, @AfterClass

JUnit5
- @Tag(String)
- @ExtendWith, @RegisterExtension
- Disabled
- @BeforeEach , @AfterEach, @BeforeAll, @AfterAll




