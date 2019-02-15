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
  - [persist](https://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#persist(java.lang.Object)
  - Persist() 메소드에 넘긴 그 엔티티 객체를 Persistent 상태로 변경
  
  EntityManager.merge()
  - [merge](https://docs.oracle.com/javaee/6/api/javax/persistence/EntityManager.html#merge(java.lang.Object)
  - Merge() 메소드에 넘긴 그 엔티티의 복사본을 만들고, 그 복사본을 다시 Persistent 상태로 변경하고 그 복사본을 반환

~~~java
@RunWith(SpringRunner.class)
@DataJpaTest
public class PostRepositoryTest {

    @Autowired
    PostRepository postRepository;

    @PersistenceContext
    private EntityManager entityManager;

    @Test
    public void save() {
        Post post = new Post();
        post.setTitle("jpa");
        Post savedPost = postRepository.save(post); //persist

        assertThat(entityManager.contains(post)).isTrue();
        assertThat(entityManager.contains(savedPost)).isTrue();
        assertThat(savedPost == post); 

        Post postUpdate = new Post();
        postUpdate.setId(post.getId());
        postUpdate.setTitle("hibernate");
        Post updatedPost = postRepository.save(postUpdate);// merge

        assertThat(entityManager.contains(updatedPost)).isTrue();
        assertThat(entityManager.contains(postUpdate)).isFalse();
        assertThat(updatedPost == postUpdate);


        List<Post> all = postRepository.findAll();
        assertThat(all.size()).isEqualTo(1);
    }

}
~~~  
  
- **jpa** 저장 사용 시 반환된 객체를 사용하는것이 좋음!!! persist()로 반환했는지, Merge()로 반환했는지에 따라 달라지기 때  
  
## 쿼리메소드
- [Query Creation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation)
- And, Or
- Is, Equals
- LessThan, LessThanEqual, GreaterThan, GreaterThanEqual
- After, Before
- IsNull, IsNotNull, NotNull
- Like, NotLike
- StringWith, EndingWith, Containing
- OrderBy
- Not, In, NotIn
- True, False
- IgnoreCase

쿼리 찾아쓰기
- 엔티티에 정의한 쿼리 찾아 사용하기 JPA Named 쿼리
- @NamedQuery
- @NamedNativeQuery
- 리포지토리 메소드에 정의한 쿼리 사용하기
- @Query
- @Query(nativeQuery=true)

## 쿼리 메소드 Sort
이전과 마찬가지로 Pageable 이나 Sort를 매개변수로 사용 할 수 있는데, @Query와 같이 사용할때 제약 사항이 있음

- Orderby 절에서 함수를 호출하는 경우에는 Sort를 사용하지 못함. 그경우에는 JpaSort.unsafe() 사용해야함
- Sort는 그안에서 사용한 프로퍼티 또는 alias가 엔티티에 없는 경우에는 예외가 발생
- JpaSort,unsafe()를 사용하면 함수 호출을 할 수 있음
- JpaSort.unsafe("LENGTH(firstname)");


## Named Parameter과 SpEL
Named Paramter
- @Query에서 참조하는 매개벼누를 ?1, ?2 이렇게 채번으로 참조하는게 아니라 이름으로
:title 이렇게 참조하는 방법은 다음과 같음

~~~java 
@Query("SELECT p FROM Post AS p WHERE p.title = :title")
List<Post> findByTitle(@Param("title") String title, Sort sort);
~~~ 

SpEL
- 스프링 표현언어
- [Spring Expression Language](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#expressions)
- @Query에서 엔티티 이름을 #{#entityName}으로 표현 할 수 있음

~~~java
@Query("SELECT p FROM #{#entityName} As p WHERE p.title = :title")
List<Post> findByTitle(@Param("title") String title, Sort sort);
~~~


## Update 쿼리 메소드
쿼리 생성하기
- find...
- count...
- delete...
- update는 어떻게하지?!!

Update 또는 Delete 쿼리 직접 정의하기
- @Modifying @Query
- 추천하지 않는다!!

~~~java

@Modifying(clearAutomatically = true, flushAutomatically = true)
@Query("UPDATE Post p SET p.title = ?2 WHERE p.id =?1")
int upadateTitle(Long id, String title);
~~~


## EntityGraph
쿼리 메소드 마다 연관관계의 Fetch모드를 설정 할 수 있음

@NamedEntityGraph
- @Entity에서 재사용 할 여러 엔티티 그룹을 정의할떄 사용함.
- 그래프 타입 설정 가능
- (기본값) FETCH: 설정한 엔티티 애트리뷰트는 EAGER 패치 나머지는 LAZY 패치
- LOAD: 설정한 엔티티 애트리뷰트는 EAGER 패치 나머지는 기본 패치 전략따름


## Projection
엔티티의 일부 데이터만 가져오기

인터페이스 기반 프로젝션
- Nested 프로젝션 가능
- Closed 프로젝션
- 쿼리를 최척화 할 수 있음. 가져오려는 애트리뷰트가 뭔지 알고 있음
- Java8의 디폴트 메소드를 사용해서 연산 할 수 있음

- Open 프로젝션
- @Value(SpEL)을 사용해서 연산을 할 수 있음.
- 스프링 빈의 메소드도 호출 가능
- 쿼리 최적화를 할수없음. SpEL을 엔티티 대상으로 사용하기때문에

클래스 기반 프로젝션
- DTO
- 롬복 @Value로 코드 줄 일 수 있음

다이나믹 프로젝션
- 프로젝션 용 메소드 하나만 정의하고 실제 프로젝션 타입은 타입 인자로 전달하기.

~~~
<T> List<T> findByPost_Id(Long id, Class<T> type);
~~~

## Specifications
에릭 에반스의 책 DDD에서 언급하는 Specification 개념을 차용한 것으로 QueryDSL의 
Predicate와 비슷함

설정하는 방법
- [Hibernate JPA 2 Metamodel Generator](https://docs.jboss.org/hibernate/stable/jpamodelgen/reference/en-US/html_single/)
- 의존성 설정
- 플러그인 설정
- IDE에 애노테이션 처리기 설정
- 코딩 시작

의존성추가
~~~
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-jpamodelgen</artifactId>
</dependency>
~~~

~~~
  <plugin>
        <groupId>org.bsc.maven</groupId>
        <artifactId>maven-processor-plugin</artifactId>
        <version>2.0.5</version>
        <executions>
            <execution>
                <id>process</id>
                <goals>
                    <goal>process</goal>
                </goals>
                <phase>generate-sources</phase>
                <configuration>
                    <processors>
                        <processor>org.hibernate.jpamodelgen.JPAMetaModelEntityProcessor</processor>
                    </processors>
                </configuration>
            </execution>
        </executions>
        <dependencies>
            <dependency>
                <groupId>org.hibernate</groupId>
                <artifactId>hibernate-jpamodelgen</artifactId>
                <version>${hibernate.version}</version>
            </dependency>
        </dependencies>
</plugin>
~~~

애노테이션 처리기 설정
Preferences -> Compiler  -> Annotation Processors
org.hibernate.jpamodelgen.JPAMetaModelEntityProcessor

~~~java
public interface CommentRepository extends JpaRepository<Comment, Long>,
JpaSpecificationExecutor<Comment>{
}
~~~

## Query by Example
QBE는 필드 이름을 작성할 필요 없이 단순한 인터페이스를 통해 동적으로 쿼리를 만드는 기능을 제공하는 사용자 친화적인 쿼리 기술

Example = Pobe+ExampleMatcher
- Probe는 필드에 어떤 값들을 갖고 있는 도메인객체
- ExampleMatcher는 Prove 에 들어있는 그 필드의 값들을 어떻게 쿼리 할 데이터와 비교할지 정의한것
- Example은 그둘을 하나로 합친것, 이걸로 쿼리를함

장점
- 별다른 코드 생성기나 애노테이션 처리가 필요없음
- 도메인 객체 리팩토링 해도 기존쿼리가 깨질 걱정하지 않아도됨
- 데이터 기술에 독립적인 API

단점
- nested 또는 프로퍼티 그룹 제약 조건을 못만든다.
- 조건이 제한적이다. 문자열은 starts/contains/end/regex가 가능하고 그밖에 
poperty는 값이 정확히 일치해야한다.

QueryByExampleExecutor  
- [Query by Example](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#query-by-example)

## 트랜잭션
스프링 데이터 JPA가 제공하는 Repository의 모든 메소드에는 기본적으로 @Transaction 이 적용되어 있습니다.

스프링 @Transactional
- 클래스, 인터페이스, 메소드에 사용 할 수 있으며, 메소드에 가장 가까운 애노테이션이 우선순위가 높다
- [Transaction 반드시 읽어볼것 그래야 설정해서 쓸수있는지 알수있음](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)

JPA 구현체로 Hibernate를 사용할때 readOnly를 사용하면 좋은점
- Flush 모드를 NEVER 로 설정하여, Dirty checking을 하지 않도록 한다.

## Auditing
스프링 데이터 JPA의 Auditing

~~~java
@CreatedDate
private Date created;

@LastModifiedDate
private Date updaated;

@CreatedBy
@ManyToOne
private Account createdBy;

@LastModifiedBy
@ManyToOne
private Account updateBy;
~~~
엔티티의 변경 시점에 언제,누가,변경했는지에 대한 정보를 기록하는 기능.

아쉽지만 이기능은 스프링부트가 자동으로 설정해주지 않음
1. 메인어플리케이션 위에 @EnableJpaAuditing 추가
2. 엔티티 클래스 위에 @EntityListeners(AuditingEntityListener.class) 추가
3. AuditorAware 구현체 만들기
4. @EnableJpaAuditing에 AuditorAware 빈 이름 설정하기

JPA의 라이프 사이클 이벤트
- [Entity listeners and Callback methods](https://docs.jboss.org/hibernate/orm/4.0/hem/en-US/html/listeners.html)
- @Prepersist
- @PreUpdate
- ...


## 마무리