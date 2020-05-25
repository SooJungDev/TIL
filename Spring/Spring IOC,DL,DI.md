## IOC란?
- 객체에 대한 제어이 스프링 컨테이너에게 넘어가면서 객체의 생명주기를 관리하는 권한 또한 컨테이너들이 전담하게됨
- 이처럼 객체의 생성에서 부터 생명주기와 관리까지 모든 객체에 대한 제어권이 바뀐것을 의미하는것이 제어의 역전 IOC

크게 두가지 방법이 있음
DL(Dependency Lookup)

DI(Dependency Injection)


## DL(Dependency Lookup)
- 의존대상(사용할 객체)를 검색(lookup) 을 통해 반환 받는 방식
- JNDI 같은 저장소에 의해 관리되고 있는 bean을 개발자들이 직접 컨테이너에서 제공하는 API 를 이용하여 Look up 하는것을 말함
    - 따라서 종속성이 생김(JNDI 의존성이 강함)
장점
- 오브젝트간의 디커플링을 해주면에서 장점

단점
- 이렇게 만들어진 오브젝트는 컨테이너 밖에서 실행 할 수 없음
- JNDI외의 방법을 사용할 경우 JNDI 관련 코드를 오브젝트내에 일일히 변경해줘야 하며 테스트하기 매우어려움
- 코드양이 매우 증가 매번 casting 해야하고 NameException 같은 checked exception을 처리하기위해서
exception 처리구조가 매우 복잡해지는 단점이 있음


## DI(Dependency Injection)식
- 의존대상(사용할 객체)을 주입을 통해 받는 방식
- 각 클래스 사이의 의존관계를 빈 설정 정보 바탕으로 container 가 자동으로 연결해줌
- 따라서 lookup 과 관련된 코드들이 오브젝트 내에 완전히 사라지고 컨테이너에 의존적이지 않은 코드를 작성 할 수 있음

주입 방식 세가지
사용할 수 있는 위치
- 생성자(스프링 4.3부터는 생략가능)
  - 불변성 Immutability
    - final 을 선언 할 수 있음 
    - 또한 생성과 동시에 만들어 지기 때문에 변할 수 없음
    - 단일 생성자에 한해 스프링 4.3버전부터는  @Autowired를 붙이지 않아도 된다.
    - 스프링 4.x 다큐멘테이션에서는 더이상 Setter Injection이 아닌 Constructor Injection을 권장한다
  - Construction Injection 에서 순환 의존성을 가질 경우 BeanCurrentlyCreationExeption을 발생시킴으로써 순환 의존성을 알 수 있다.
  
- 세터
 - 선택적인 의존성을 사용 할 때 유용하다 상황에 따라 의존성 주입이 가능하다
 - 스프링 3.X 다큐멘테이션에서는 setter injection 을 추천했음
  Setter Injection을 사용한다면, 합리적인 디폴트를 부여할 수 있고 선택적인(optional) 의존성을 사용할 때만 사용해야한다고 말한다
 
- 필드  
 - 지양해야함 그이유
   - 단일 책임의 원칙 위반
   - 의존성이 숨는다
   - 객체가 변할수 있음


참고사이트
- [[Spring]필드 주입(Field Injection) 대신 생성자 주입(Constructor Injection)을 사용해야 하는 이유](https://zorba91.tistory.com/238)
- [[Spring Framework] IOC, DI, DL](https://m.blog.naver.com/PostView.nhn?blogId=wwwkang8&logNo=220985550237&proxyReferer=https%3A%2F%2Fwww.google.com%2F&view=img_4)
- [Spring Framework와 [IOC / DL / DI] 정의](https://madnix.tistory.com/entry/Spring-Framework%EC%99%80-IOC-DL-DI-%EC%A0%95%EC%9D%98)
- [Dependency Lookup](http://javaexplorer03.blogspot.com/2016/02/dependency-lookup.html)
