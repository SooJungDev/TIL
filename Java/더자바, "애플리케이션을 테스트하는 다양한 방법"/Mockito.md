## Mockito 소개
- Mock : 진 객체와 비슷하게 동작하지만 프로그래머가 직접 그 객체의 행동을 관리하는 객체
- Mockito: Mock 객체를 쉽게 만들고 관리하고 검증 할 수 있는 방법을 제공한다.

테스트를 작성하는 자바 개발자 50% + 사용하는 Mock 프레임워크
- [devecosystem java](https://www.jetbrains.com/lp/devecosystem-2019/java/)

현재 최신 버젼 3.1.0

단위 테스트에 고찰
- [Unit Test](https://martinfowler.com/bliki/UnitTest.html)
- 행동의 단위로 본다. 일하는 사람과 맞추면 된다. 단위테스트의 단위를 정할지 mock 은 어떤 범위까지

대체제
- [EasyMock](http://easymock.org/)
- [JMocK](http://jmock.org/)

## Mockito 시작하기
- 스프링부트 2.2+ 프로젝트 생성시 spring-boot-starter-test 에서 자동으로 Mockito 추가 해 줌

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
- Mock을 만드는 방법
- Mock이 어떻게 동작해야 하는지 관리하는 방법
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

~~~java
    @Test
    public void createNewStudyService(@Mock MemberService memberService,
                                      @Mock StudyRepository studyRepository) {

        StudyService studyService = new StudyService(memberService, studyRepository);
        assertNotNull(studyService);

        Member member = new Member();
        member.setId(1L);
        member.setEmail("soojungchoi@email.com");

        when(memberService.findById(any()))
                .thenReturn(Optional.of(member))
                .thenThrow(new RuntimeException())
                .thenReturn(Optional.empty());

        Optional<Member> byId = memberService.findById(1L);
        assertEquals("soojungchoi@email.com",byId.get().getEmail());

        assertThrows(RuntimeException.class, ()->{
            memberService.findById(2L);
        });

        assertEquals(Optional.empty(), memberService.findById(3L));


    /*    Study study = new Study(10, "java");

        Optional<Member> findByID = memberService.findById(2L);
        assertEquals("soojungchoi@email.com", findByID.get().getEmail());

        doThrow(new IllegalArgumentException()).when(memberService).validate(1L);
        assertThrows(IllegalArgumentException.class, ()->{
            memberService.validate(1L);
        });

        memberService.validate(2L);

        // studyService.createNewStudy(1L, study);*/

    }
~~~

Mock 객체를 조작해서
- 특정한 매개변수를 받은 경우 특정한 값을 리턴하거나 예외를 던지도록 만들 수 있다
    - [How about some stubbing?](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#2)    
    - [Argument matchers](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#3)
~~~java
 //stubbing using built-in anyInt() argument matcher
 when(mockedList.get(anyInt())).thenReturn("element");

 //stubbing using custom matcher (let's say isValid() returns your own matcher implementation):
 when(mockedList.contains(argThat(isValid()))).thenReturn(true);

 //following prints "element"
 System.out.println(mockedList.get(999));

 //you can also verify using an argument matcher
 verify(mockedList).get(anyInt());

 //argument matchers can also be written as Java 8 Lambdas
 verify(mockedList).add(argThat(someString -> someString.length() > 5));

~~~
- Void 메소드 특정 매개변수를 받거나 호출된 경우 예외를 발생 시킬 수 있다.
    - [Stubbing void method with exceptions](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#5)
- 메소드가 동일한 매개변수로 여러번 호출 될 떄 각기 다르게 행동하도록 조작 할 수도 있다
    - [Stubbing consecutive calls](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#10)
~~~ java
 when(mock.someMethod("some arg"))
   .thenThrow(new RuntimeException())
   .thenReturn("foo");
~~~
    

## Mock 객체 Stubbing 연습문제
다음 코드의 //TODO에 해당하는 작업을 코딩으로 채워 넣으세요.
~~~java
Study study = new Study(10, "테스트");

// TODO memberService 객체에 findById 메소드를 1L 값으로 호출하면  Optional.of(member) 객체를 리턴하도록 Stubbing
when(memberService.findById(1L)).thenReturn(Optional.of(member));

// TODO studyRepository 객체에 save 메소드를 study 객체로 호출하면 study 객체 그대로 리턴하도록 Stubbing
when(studyRepository.save(study))).thenReturn(study);

studyService.createNewStudy(1L, study);

assertNotNull(study.getOwner()); 
assertEquals(member, study.getOwner());
~~~

## Mock 객체 확인
Mock 객체가 어떻게 사용이 됐는지 확인할 수 있다.
- 특정 메소드가 특정 매개변수로 몇번 호출 되었는지, 최소 한번은 호출 됐는지, 전혀
호출되지 않았는지
    - [Verifying exact number of invocations](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#exact_verification)
- 어떤 순서대로 호출했는지
    - [Verification in order](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#in_order_verification)
- 특정 시간 이내에 호출됐는지
    - [Verification with timeout](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#verification_timeout)
- 특정 시점 이후에 아무 일도 벌어지지 않았는지
    - [Finding redundant invocations](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#finding_redundant_invocations)
    
## Mockito BDD 스타일 API
[BDD](https://en.wikipedia.org/wiki/Behavior-driven_development): 애플리케이션이 어떻게 "행동"해야 하는지에 대한 공통된 이해를 구성하는 방법으로, TDD에서 창안했다

행동에 대한 스팩
- Title
- Narrative
    - As a / I want/ so that
- Acceptance criteria 
    - Given(주어진 상황) / When(뭔가를 하면) / Then(이럴 것이다)
 
Mockito 는 BddMockito 라는 클래스를 통해 BDD 스타일의 API 를 제공한다
when -> Given
~~~java
given(memberService.findById(1L)).willReturn(Optional.of(member));
given(studyRepository.save(study)).willReturn(study);
~~~

verify -> Then
~~~java
then(memberService).should(times(1)).notify(study);
then(memberService).shouldHaveNoMoreInteractions();
~~~

참고사이트
- [Class BDDMockito](https://javadoc.io/static/org.mockito/mockito-core/3.2.0/org/mockito/BDDMockito.html)
- [BDD style verification](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#BDD_behavior_verification)
 
## Mockito 연습 문제
다음 StudyService 코드에 대한 테스트를 Mockito를 사용해서 Mock 객체를 만들고 Stubbing과 Verifying을 사용해서 테스트를 작성하세요.

StudyService.java
~~~java
public Study openStudy(Study study) {
    study.open();
    Study openedStudy = repository.save(study);
    memberService.notify(openedStudy);
    return openedStudy;
}
~~~

~~~java
@DisplayName("다른 사용자가 볼 수 있도록 스터디를 공개한다.")
@Test
void openStudy() {
    // Given
    StudyService studyService = new StudyService(memberService, studyRepository);
    Study study = new Study(10, "더 자바, 테스트");
    // TODO studyRepository Mock 객체의 save 메소드를호출 시 study를 리턴하도록 만들기.
    given(studyRepository.save(study)).willReturn(study);


    // When
    studyService.openStudy(study);

    // Then
    // TODO study의 status가 OPENED로 변경됐는지 확인
    assertEquals(StudyStatus.OPEND, study.getStatus());
    // TODO study의 openedDataTime이 null이 아닌지 확인
    assertNotNull(study.getOpenedDateTime());
    // TODO memberService의 notify(study)가 호출 됐는지 확인.
    then(memberService).should().notify(study);
}
~~~
