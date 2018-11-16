## Open Session In View Pattern
- 해당 패턴은 객체-관게 매핑(ORM Object-Relational Mapping)의 사용으로 등장하게 된 패턴 
- Open Sessiong In view는 영속성 컨텍스트 뷰 렌더링이 끝나는 시점까지 개방한 상태로 유지하는 것

## 영속성 컨텍스트 
- Hibernate는 도메인 레이어 객체들이 하부의 데이터 저장소와 영속성 매커니즘에 대해 알지 않아도되는 투명한 
영속성을 제공하는 비침투적인(nonintrusive) 프레임워크임
- 영속성과 관련된 모든 관심사는 도메인 객체로 부터 격리된 관리자 객체에 의해 투명하게 처리됨
- 관리자 객체의 기능을 담당하는 객체가 Hibernate의 Session 객체는 영속성 컨텍스트를 포함함
- Transaction 는 쪼갤 수 없는 업무처리의 단위 
- 영속성 컨텍스트는 transaction 과 1:1로 연결됨
- 하나의 transaction 동안 수정된 객체의 모든 상태는 영속성 컨텍스트 내에 저장되어 transaction이 종료 될때 데이터 저장소에 동기화
~~~ java
@Transactional
public void moveTeam(long memberIdx, Team team){
    Member member = memberRepository.findOne(memberIdx);
    member.moveTeam(team);
}
~~~
- @Transactional 어노테이션에 의해 자동으로 session 이 열림 
- 조회를 통해 가져온 member 객체는 영속성 콘텍스트에 의해 캐쉬됨
- Transaction 동안 Member 객체의 변경사항이 추적되고 이떄 Member 객체는 영속성을 갖는다고 말함
- moveTeam에 의해 영속성을 갖는 Member 객체가 변경되는 것을 영속성 컨텍스트가 감지
- Transaction 이 종료되는 시점인 메서드 종료 시점에 영속성 컨텍스트가 캐쉬하고있는 객체 변경 상태를 저장소와 동기화
- 이때 자동으로 실행되게됨

## 영속성을 갖는 객체의 상태
- Persistence 
  - 데이터베이스 식별자(주키)를 가지며 작업단위내에서 영속성에 의해 관리되는 상태
  - Transaction 안에서 조회,저장,수정등을 통해 가져온 영속성이 부여된 상태 
  - Transient 상태의 객체도 강제로 Persistence 상태로 전환 시킬 수 있음 
  
- Detached
  - 데이터베이스 식별자를 가지지만 영속성 컨텍스트로 부터 분리되어 더이상 데이터베이스와 동기화가 보장되지 않는 상태
  - Session.close() 메소드는 Session을 닫고 영속성 컨텍스트를 포함하고 있는 모든 자원을 반환
  - 관리하에 있는 모든 영속 인스턴스를 Detached 상태로 변경시킴
  - hibernate는 Detached 상태의 객체에 대해서는 변경사항을 추적하지 않으며, 따라서 데이터베이스와의 동기화 수행하지 않음
  
- Transient
  - 아무 상태도 가지지 않는 상태 (무 상태를 정의하기 위한 상태)
  - new 연산자를 사용하여 생성된 객체는 곧바로 Persistence 상태 되지않고 Transient 상탤르 갖게됨

- Removed
   - 제거될 상태
   - Transaction이 종료되는 시점에 삭제될 객체는 Removed 상태를 가짐
 
## 지연로딩(Lazy Loading)
- 패치 전략 : 쿼리수행과 관련된 객체와 연관관계를 맺고있는 객체나 컬렉션을 어느 시점에 가져올지에 대한 전략

- JPA에 정의하는 전략을 두가지 
  - EAGER : 데이터를 즉시 가져오는 전략, 즉시 로딩
  - LAZY : 데이터가 처음 액세스 될때 가져오는 전략으로 지연로딩
  
-  즉시 로딩(EAGER)
   - @ManyToOne
   - @OneToOne
 - 지연로딩
   - @OneToMany
   - @ManyToMany
 프록시 객체는 주로 연관된 데이터를 지연로딩 할 때 사용됨
 
 프록시
 지연 패치 전략에 따라 연관 관계를 맺고 있는 객체와 컬렉션에는 실제 entity 대신 실제 객체처럼
 위장한 프록시(proxy) 객체가 생성됨
 프록시 초기화라 하고 처음 사용 될때 한번만 초기화됨  초기화 되면 프록시를 통해 실제 엔티티에 접근가능하게됨  
 
 ~~~
 @Entity
 public class Team{
    @Id
    @GeneratedValue
    private Long idx;
    
    @Column(nullable = false, length =20)
    private String name;
    
    @OneToMany(mappedBy = "team")
    private List<Member> members;
 }
 JPA의 기본 Select을 통해 Team 조회하였을때 연관관계를 맺고있는 members List 객체는 지연페치의 대상이되어
 Proxy 객체로 채워짐
 ~~~
## Open Session In View 패턴
- 뷰 렌더링 시점에 영속성 컨텍스트가 존재하지 않기 떄문에 Detached 객체의 프록시를 초기화 할 수 없다면
영속성 컨텍스트를 오픈된 채 뷰 렌더링 시점까지 유지하자는 것, 즉 작업단위를 요청 시작 시점부터 뷰렌더링 완료 시점까지로 확장하는 것
## 참고사이트
  - [OSIVP](http://kingbbode.tistory.com/27)
 