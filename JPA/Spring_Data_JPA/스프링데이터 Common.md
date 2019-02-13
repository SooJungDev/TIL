## 스프링데이터 JPA 활용 파트 소개

- 스프링데이터 : SQL & NoSQL 저장소 지원 프로젝트의 묶음.
- 스프링데이터 Common : 여러 저장소 지원 프로젝트 공통 기능제공
- 스프링 데이터 REST : 저장소의 데이터를 하이퍼미디어 기반 HTTP 리소스로(REST API로) 제공하는 프로젝트
- 스프링 데이터 JPA : 스프링 데이터 Common이 제공하는 기능에 JPA 관련 기능 추가
- [Spring Data](http://spring.io/projects/spring-data)


## Repository

Repository  

CrudRepository  

PagingAndSoringRepository  

JpaRepository

## Repository 인터페이스 정의하기
Repository 인터페이스로 공개할 메소드를 직접 일일히 정의하고 싶다면

특정 리포지토리 당
- @RepositoryDefinition

~~~java
@RepositoryDefinition(domainClass = Comment.class, idClass = Long.class)
public interface CommentRepository{
    
    Comment save(Comment comment);
    
    List<Comment> findAll();
}
~~~

공통 인터페이스 정의
- @NoRepositoryBean

~~~java
@NoRepositoryBean
public interface MyRepository<T,ID extends Serializable> extends Repository<T, ID>{
    <E extends T> E save(E entity);
    
    List<T> findAll();
}
~~~

## Null 처리하기
- 스프링 데이터 2.0 부터 자바8의 Optional 지원
    - Optional<Post> findById(Long id);
    - 콜렉션은 Null을 리턴하지 않고, 비어있는 콜렉션을 리턴함
    
스프링 프레임워크 5.0부터 지원하는 Null 애노테이션 지원.  
- @NonNullApi, @NonNull, @Nullable
- 런타임 체크 지원함.
= JSR 305 애노테이션을 메타 애노테이션으로 가지고 있음(IDE 및 빌드를 지원)  

인텔리J 설정
- Build, Execution, Deployment
    - Compiler
    - Add runtime assertion for notnull-annotated methods and parameters
    
    
## 쿼리만들기 개요
스프링 데이터 저장소의 메소드 이름으로 쿼리만드는 방법
- 메소드 이름을 분석해서 쿼리만들기(CREATE)
- 미리 정의해둔 쿼리 찾아 사용히가(USE_DECLARED_QUERY)    
- 미리 정의한 쿼리 찾아보고 없으면 만들기(CREATE_IF_NOT_FOUND)

쿼리 만드는 방법  
- 리턴타입{접두어}{도입부}By{프로퍼티 표현식}(조건식)[(And|OR){프로퍼티 표현식}(조건식)]{정렬조건}(매개변수)

접두어
- Find,Get,Query,Count,...

도입부
- Distinct,First(N),Top(N)

프로퍼티 표현식
- Person.Address.ZipCode => find(Person)ByAddress_ZipCode(...)

조건식
- IgnoreCase,Between,LessThan,GreaterThan, Like, Contains

정렬조건
- OrderBy{프로퍼티}Asc|Desc

리턴타입
- E, Optional<E>, List<E>, Page<E>, Slice<E>, Stream<E>

매개변수
- Pageable, Sort

쿼리 찾는 방법
- 메소드 이름으로 쿼리를 표현하기 힘든 경우 사용.
- 저장소 기술에 따라 다름
- JPA:@Query @NamedQuery

## 쿼리만들기 실습

기본예제
~~~java
List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastName);

//distinct
List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);

//ignoring case
List<Person> findByLastnameIgnoreCase(String lastname);
//ignoring case
List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);
~~~

정렬
~~~java
List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
~~~

페이징
~~~java
Page<User> findByLastname(String lastname, Pageable pageable);
Slice<User> findByLastname(String lastname, Pageable pageable);
List<User> findByLastname(String lastname, Sort sort);
List<User> findByLastname(String lastname, Pageable pageable);
~~~

스트리밍
~~~java
Stream <User> readAllByFirstnameNotNull();
~~~
- try-with-resource 사용할것 (Stream을 다 쓴다음에 close() 해야 함)

## 비동기쿼리

~~~java
@Async Future<User> findByFirstname(String firstname);
@Async CompletableFuture<User> findOneByFirstname(String firstname);
@Async ListenableFuture<User> findOneByLastname(String lastname);
~~~
- 해당 메소드 스프링 TaskExecutor에 전달해서 별도의 쓰레드에서 실행함.
- Reactive랑 다른것임

권장하지 않는 이유
- 테스트 코드 작성이 어려움
- 코드 복잡도 증가
- 성능상 이득이 없음
    - DB 부하는 결국 같고
    - 메인 쓰레드 대신 백그라운드 쓰레드가 일하는 정도의 차이
    - 단 백그라운드로 실행하고 결과를 받을 필요가 없는 작업이라면 @Async를 사용해서 응답속도를 향상 시킬 수는 있음

ListenableFuture
- Nonblocking, async

## 커스텀 리포지토리

쿼리메소드(쿼리새성과 쿼리 찾아쓰기)로 해결이 되지 않는 경우 직접 코딩으로 구현가능.  
- 스프링 데이터 리포지토리 인터페이스에 기능추가
- 스프링 데이터 리포지토리 기본기능 덮어쓰기 가능
- 구현 방법
    - 커스텀 리포지토리 인터페이스 정의
    - 인터페이스 구현 클래스 만들기(기본 접미어는 Impl)
    - 엔티티 리포지토리에 커스텀 리포지토리 인터페이스 추가

기능추가하기 
~~~java
public interface PostCustomRepository<T> {
    List<Post> findMypost();

    void delete(T entity);
}

~~~ 

~~~ java
package me.crystal.demojpa3.post;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import javax.transaction.Transactional;
import java.util.List;

@Repository
@Transactional
public class PostCustomRepositoryImpl implements PostCustomRepository<Post> {

    @Autowired
    EntityManager entityManager;
    @Override
    public List<Post> findMypost() {
        System.out.println("custom findMypost");
        return entityManager.createQuery("select p from Post As p",Post.class)
                .getResultList();
    }

    @Override
    public void delete(Post entity) {
        System.out.println("custom delete");
        entityManager.remove(entity);
    }
}
~~~
기본기능 덮어쓰기  


접미어 설정하기     
~~~java
@SpringBootApplication
@EnableJpaRepositories(repositoryImplementationPostfix = "default")
public class Application {
~~~
    
## 기본 리포지토리 커스터마징
모든 리포지토리에 공통적으로 추가 하고싶은 기능이 잇거나 덮어쓰고 싶은 기본 기능이 있다면  

1. JpaRepository를 상속 받는 인터페이스 정의
    - @NoRepositoryBean
2. 기본 구현체를 상속받는 커스텀 구현체 만들기
3. @EnableJpaRepositories에 설정
    - repositoryBaseClass
    
~~~ java 
@NoRepositoryBean
public interface MyRepository<T, ID extends Serializable> extends JpaRepository<T,ID>{
    
    boolean contains(T entity);
}
~~~   

~~~ java
public class SimpleMyRepository<T, ID extends Serializable> extends SimpleJpaRepository<T, ID>
 implements MyRepository<T,ID>{
    
    private EntityManager entityManager;
    
    public SimpleMyRepository(JpaEntityInformation<T, ?> entityInformation, EntityManager entityManager){
        super(entityInformation, entityManager);
        this.entityManager = entityManager;
    } 
    
    @Override
    public boolean contains(T entity){
        return entityManager.contains(entity);
    }
 
}
~~~ 


~~~ java
@EnableJpaRepositories(repositoryBaseClass = SimpleMyRepository.class)
~~~


~~~ java
public interface PostRepository extends MyRepository<Post, Long>{
}
~~~


## 도메인 이벤트
도메인 고나련 이벤트를 발생시키기

스프링 프레임워크의 이벤트 관련기능
- [Standard and Custom Events](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#context-functionality-events)
- ApplicationContext extends ApplicationEventPublisher
- 이벤트: extends ApplicationEvent
- 리스너
    - implements ApplicationListener<E extends ApplicationEvent>
    - @EventListener
    
스프링 데이터의 도메인 이벤트 Publisher
- @DomainEvents
- @AfterDomainEventPublication
- extends AbstractAggregationRoot<E>
- 현재는 save() 할때만 발생합니다.

## QueryDSL
~~~java
 findByFirstNameIngoreCaseAndLastNameStartsWithIgnoreCase(String firstName, String lastName)
~~~
- 위와같은 메소드 너무길어서 어지러움 

여러 쿼리 메소드는 대부분 두가지중 하나.  
- Optional<T> findOne(Predicate): 이런 저런 조건으로 무언가 하나를 찾음
- List<T> | Page<T> | ...findAll(Predicate) : 이런저런 조건으로 무언가 여러개 찾는다
- [QuerydslPredicateExecutor 인터페이스](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/querydsl/QuerydslPredicateExecutor.html)

QueryDSL
- [QueryDSL](http://www.querydsl.com/)
- 타입 세이프한 쿼리 만들 수 있게 도와주는 라이브 러리
- JPA, SQL, MongoDB, JDO, Lucene, Collection 지원
- [QueryDSL JPA 연동 가이드](http://www.querydsl.com/static/querydsl/4.1.3/reference/html_single/#jpa_integration)

스프링 데이터 JPA + QueryDSL
- 인터페이스: QuerydslPredicateExecutor<T>
- 구현체: QuerydslPredicateExecutor<T>

연동방법
- 기본 리포지토리 커스터마이징 안했을때 (쉬움)
- 기본 리포지토리 커스터마이징 했을때 (해맬 수 있음)

의존성 추가
~~~
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-apt</artifactId>
</dependency>
<dependency>
    <groupId>com.querydsl</groupId>
    <artifactId>querydsl-jpa</artifactId>
</dependency>
~~~

~~~
 <plugin>
    <groupId>com.mysema.maven</groupId>
    <artifactId>apt-maven-plugin</artifactId>
    <version>1.1.3</version>
    <executions>
        <execution>
            <goals>
                <goal>process</goal>
            </goals>
            <configuration>
                 <outputDirectory>target/generated-sources/java</outputDirectory>
                <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
            </configuration>
        </execution>
    </executions>
</plugin>
~~~

~~~java
public interface AccountRepository extends JpaRepository<Account, Long>, QuerydslPredicateExecutor<Account>{
}
~~~

## Web: 웹지원 기능소개
스프링 데이터 웹 지원 기능 설정
- 스프링 부트를 사용하는 경우에.. 설정할 것이 없음(자동 설정)
- 스프링 부트 사용하지 않는 경우

~~~java
@Configuration
@EnableWebMvc
class WebConfiguration {}
~~~

제공하는 기능
- 도메인 클래스 컨버터
- @RequestHandler 메소드에서 Pageable과 Sort 매개변수 사용
- Page 관련 HATEOAS 기능 제공
    - PagedResourcesAssembler
    - PagedResource
- Payload 프로젝션
    - 요청으로 들어오는 데이터 중 일부만 바인딩 받아오기
    - @ProjectedPayload, @XBRead, @JsonPath
- 요청 쿼리 매개변수를 QueryDSLdml Predicate로 받아오기
    - ?firstname=Mr&lastname=White => Predicate     
     
## Web: DomainClassConverter

스프링 Converter
- [Converter](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/convert/converter/Converter.html)
- Formatter도 들어본것같은데?...

~~~java
    @GetMapping("/post/{id}")
    public String getAPost(@PathVariable Long id){
        Optional<Post> byId = PostRepository.findById(id);
        Post post = byId.get();
        return post.getTitle();
    }
~~~


~~~java
    @GetMapping("/posts/{id}")
    public String getAPost(@PathVariable("id") Post post){
        return post.getTitle();
    }
~~~

## Web: Pageable 과 Sort 매개변수
스프링 MVC HandlerMethodArgumentResolver
- 스프링 MVC 핸들러 메소드의 매개변수로 받을 수 있는 객체를 확장 하고 싶을때 사용하는 인터페이스
- [Interface HandlerMethodArgumentResolver](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/method/support/HandlerMethodArgumentResolver.html)

페이징과 정렬 관련 매개변수
- page: 0부터 시작
- size: 기본값 20.
- sort: property.property(,ASC|DESC)
- 예) sort=created, desc&sort=title (asc가 기본값)

## Web: HATEOAS
Page를 PagedResource로 변환하기
- 일단 HATEOAS 의존성 추가(starter-hateoas)
- 핸들러 매개 변수로 PagedResourcesAssembler



## 마무리

- 스프링 데이터 Repository
- 쿼리 메소드
    - 메소드 이름 보고 만들기
    - 메소드 이름 보고 찾기
- Repository 정의하기
    - 내가 쓰고 싶은 메소드만 골라서 만들기
    - Null 처리
- 쿼리 메소드 정의하는 방법
- 리포지토리 커스터 마이징
    - 리포지토리 하나 커스터 마이징
    - 모든 리포지토리의 베이스 커스터마이징
- 도메인 이벤트 Publish
- 스프링 데이터 확장 기능
    - QueryDSL 연동
    - 웹지원
