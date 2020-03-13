## JPA N+1 문제 및 해결방안
- JPA N+1 문제 발생하는 경우 @OneToMany, @ManyToMany 기본 패치 전략이 Lazy 일때 발생
- 하위 엔티티들을 첫쿼리 실행시 한번에 가져오지 않고 Lazy Loading 으로 필요한 곳에서 사용되어 있는 쿼리에서 발생함


해결방안
1.Join Fetch
~~~java
/**
* 1. join fetch를 통한 조회
*/
@Query("select a from Academy a join fetch a.subjects")
List<Academy> findAllJoinFetch();
~~~
- 하위의 하위 Entity 까지 아주 쉽게 가져오게 되어있다.
- 불필요한 쿼리문이 추가되는 단점

2.@EntityGraph
~~~java
/**
* 2. @EntityGraph
*/
@EntityGraph(attributePaths = "subjects")
@Query("select a from Academy a")
List<Academy> findAllEntityGraph();
~~~
- @EntityGraph의 attributePaths 에 쿼리 수행시 바로 가져올 필드명을 지정하면 Lazy 가 아닌 Eager 조회로 가져오게 됩니다.
- 쿼리 손상없이 Eager/Lazy 핃드를 정의하고 사용 할 수 있게 됨

join fetch와 @EntityGraph 사용시 주의사항
- **Join Fetch는 inner join, Entity Graph 는 Outer Join**
- 공통적으로 카테시안 곱 발생하여 subject 수 만큼 Academy 가 중복하여 발생

위에 같은 주의사항 해결 방안은 두가지가 있음
1. 일대다 필드 타입을 Set 으로 선언
    - Set 은 중복을 허용하지 앟는 자료구조이기 때문에 중복 등록 되지않음
~~~
@OneToMany(cascade = CascadeType.ALL)
@JoinColumn(name="academy_id")
private Set<Subject> subjects = new LinkedHashSet<>();
~~~

2. distinct 를 사용하여 중복을 제거하는 것
~~~
@Query("select DISTINCT a from Academy a join fetch a.subjects s join fetch s.teacher")
List<Academy> findAllWithTeacher()
~~~

@NamedEntityGraphs?
- @NamedEntityGraphs 같은 경우 Entity 관련해서 모든 설정 코드 추가
  - 참고사이트에서는 Entity가 해야하는 책임에 포함되지 않는다고 생각하신다고 하심

로직의 패치 전략을 어떻게 해야된다는것은 서비스/레파지토리에서 결정해야될 일!!

참고사이트
- 아래 정말 잘 설명 해 놓으심!!!!!!! 
- [JPA N+1 문제 및 해결방안](https://jojoldu.tistory.com/165)
- [MultipleBagFetchException 발생시 해결 방법](https://jojoldu.tistory.com/457)
