## JPA Repository
@EnableJpaRepositories
- 스프링부트 사용할 때는 사용하지 않아도 자동설정됨.
- 스프링 부트 사용하지 않을때는 @Configuration과 같이 사용.

@Repository 애노테이션을 붙여야 하나 말아야 하나..
- 안붙여도 됨
- 이미 붙어있고, 붙여봤자 중복일뿐

스프링 @Repository
- SQLException 또는 JPA 관련 예외를 스프링의 DataAccessException 으로 반환해줌

## 엔티티 저장하기
JpaRepository의 Save()는 단순히 새 엔티티를 추가하는 메소드아님
- Transient 상태의 객체가 EntityManager.persist()
- Detached 상테의 객체라면 EntityManager.merge()

Transient 인지 Detached 인지 어떻게 판단하는가?
- 엔티티의 @Id 프로퍼티를 찾는다.
- 해당 프로퍼티가 null이면 Transient 상태로 판단함
- id가 null이 아니면 Detached 상태로 판단한다.
- 엔티티가 Persistable 인터페이스를 구현하고있다면 isNew() 메소드에 위임한다
- JpaRepositoryFactory를 상속받는 클래스를 만들고 getEntityInformation()을 오버라이딩해서
  자신이 원하는 판단 로직을 구현 할 수도 있음
  
  EntityManager.persist()
  - [persist](https://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#persist(java.lang.Object))
  - Persist() 메소드에 넘긴 그 엔티티 객체를 Persistent 상태로 변경
  
  
  ## 쿼리메소드
  
  ## 쿼리 메소드 Sort
  
  ## Named Parameter과 SpEL
  
  ## Update 쿼리 메소드
  
  ## EntityGraph
  
  ## Projection
  
  ## Specifications
  
  ## Query by Example
  
  ## 트랜잭션
  
  ## Auditing
  
  ## 마무리