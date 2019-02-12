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

~~~

정렬
~~~java

~~~

페이징
~~~java

~~~

스트리밍
~~~java

~~~

## 비동기쿼리

## 커스텀 리포지토리

## 기본 리포지토리 커스터마징

## 도메인 이벤트

## QueryDSL

## Web: 웹지원 기능소개

## Web: DomainClassConverter

## Web: Pageable 과 Sort 매개변수

## Web: HATEOAS

## 마무리