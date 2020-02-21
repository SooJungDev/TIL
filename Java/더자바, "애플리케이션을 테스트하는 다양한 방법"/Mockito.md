## Mockito 소개
- Mock : 진자 객체와 비슷하게 동작하지만 프로그래머가 직접 그 객체의 행동을 관리하는 객체
- Mockito: Mock 객체를 쉽게 만들고 관리하고 검증 할 수 있는 방법을 제공한다.

테스트를 작성하는 자바 개발자 50% + 사용하는 Mock 프레임워크
- [devecosystem java](https://www.jetbrains.com/lp/devecosystem-2019/java/)

현재 최신 버젼 3.1.0

단위 테스트에 고찰
- [Unit Test](https://martinfowler.com/bliki/UnitTest.html)

대체제
- [EasyMock](http://easymock.org/)
- [JMocK](http://jmock.org/)

## Mockito 시작하기
- 스프링부트 2.2+ 프로젝트 생성시 spring-boot-starter-test에서 자동으로 Mockito 추가 해 줌

스프링부트 쓰지 않는다면 의존성 직접 추가
~~~
<dependency>    
    <groupId>org.mockito</groupId> 
    <artifactId>mockito-core</artifactId>
    <version>3.1.0</version> 
    <scope>test</scope>
</dependency>

<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId> 
    <version>3.1.0</version>
    <scope>test</scope>
</dependency>
~~~

다음 세가지만 알면 Mock을 활용한 테스트를 쉽게 작성 할 수 있다.
- Mock 을 만드는 방법
- Mock 이 어떻게 동작해야 하는지 관리하는 방법
- Mock의 행동을 검증하는 방법

[Mockito 레퍼런스](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html)

## Mock 객체 만들기
Mockito.mock() 메소드로 만드는 방법
~~~java
MemberService memberService = mock(MemberService.class);
StudyRepository studyRepository = mock(StudyRepository.class);
~~~

@Mock 애노테이션으로 만드는 방법
- JUnit 5 extendsion 으로 MockitoExtension 을 사용해야 한다.
- 필드
- 메소드 매개변수

~~~java
@ExtendWith(MockitoExtension.class)
class StudyServiceTest {
    @Mock MemberService memberService;

    @Mock StudyRepository studyRepository;
}
~~~

~~~java
@ExtendWith(MockitoExtension.class)
class StudyServiceTest {

   @Test
   void createStudyService(@Mock MemberServide memberService, @Mock StudyRepository studyRepository) {
        StudyService studyService = new StudyService(memberService, studyRepository);
        assertNotNull(studyServide);
   }

}
~~~

## Mock 객체 Stubbing
모든 Mock 객체의 행동
- Null을 리턴한다. (Optional 타입은 Optional.empty 리턴)
- Primitive 타입은 기본 Primitive 값
- 콜렉션은 비어있는 콜렉션.
- Void 메소드는 예외를 던지지 않고 아무런 일도 발생하지 않는다.

Mock 객체를 조작해서
- 특정한 매개변수를 받은 경우 특정한 값을 리턴하거나 예외를 던지도록 만들 수 있다
    - [How about some stubbing?](https://javadoc.io/doc/org.mockito/mockito-core/3.2.4/index.html)
    - [Argument matchers](https://javadoc.io/doc/org.mockito/mockito-core/latest/index.html)
- Void 메소드 특정 매개변수를 받거나 호출된 경우 예외를 발생 시킬 수 있다.
    - [Stubbing void method with exceptions](https://javadoc.io/doc/org.mockito/mockito-core/latest/index.html)
- 메소드가 동일한 매개변수로 여러번 호출 될 떄 각기 다르게 행동하도록 조작 할 수도 있다
    - [Stubbing consecutive calls]()

## Mock 객체 Stubbing 연습문제
다음 코드의 //TODO에 해당하는 작업을 코딩으로 채워 넣으세요.
~~~java
Study study = new Study(10, "테스트");

// TODO memberService 객체에 findById 메소드를 1L 값으로 호출하면 
Optional.of(member) 객체를 리턴하도록 Stubbing
// TODO studyRepository 객체에 save 메소드를 study 객체로 호출하면 study 객체 
그대로 리턴하도록 Stubbing

studyService.createNewStudy(1L, study);

assertNotNull(study.getOwner()); 
assertEquals(member, study.getOwner());
~~~


Mock 객체가 어떻게 사용이 됐는지 확인할 수 있다.
- 특정 메소드가 특정 매개변수로 몇번 호출 되었는지, 최소 한번은 호출 됐는지, 전혀
호출되지 않았는지
    - Verifying exact number of invocations
- 어떤 순서대로 호출했는지
    - Verification in order
- 특정 시간 이내에 호출됐는지
    - Verification with timeout
- 특정 시점 이후에 아무 일도 벌어지지 않았는지
    - Finding redundant invocations