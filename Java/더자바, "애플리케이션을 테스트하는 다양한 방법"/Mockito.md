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

}

~~~