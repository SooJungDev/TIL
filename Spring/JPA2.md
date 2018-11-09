## jpa 설정
~~~
  jpa:
    show-sql: true #사용되는 sql문을 보여줄지 선택
    database: mysql
    generate-ddl : false #ddl 사용시 데이터베이스의 고유의 기능을 사용할지 선택
    hibernate:
      ddl-auto: update
~~~
- spring.jpa.hibernate.ddl-auto
  - Data Definition Language 를 처리하는 옵션
  - create: 기존 테이블 삭제후 다시생성 
  - create-drop : create 와 동일하지만 종료시점에 테이블 drop
  - update: 변경된 부분만 반영 
  - none : 사용하지 않음

## JPA 처리를 담당하는 Repository 인터페이스 설계
~~~ java
 Repository<T,ID>
 CrudRepository<T,ID>
 PagingAndSortingRepository<T,ID>
 JpaRepository<T,ID>
~~~
- Repository가 가장 상위
- 인터페이스는 <T,ID> 두개의 제너릭 타입을 사용하는 것을 볼 수 있는데, T는 엔티티의 타입 클래스이고,ID는 primaryKey 값임
- ID는 반드시(java.io.Serializable) 인터페이스 타입
- 기본적인 CRUD 사용시 CrudRepository<T,ID>를 많이 사용하는 편
- 혹은 페이징 처리, 검색등이 가능한 pagingAndSortingRepository<T,ID> 
- JPA 에 특화된 몇개의 기능을 가능 JpaRepository<T,ID> 사용 


## 참고사이트
  - [JPA CRUD 간단한 예제](http://onemooon.tistory.com/entry/JPA-%EA%B0%84%EB%8B%A8%ED%95%9C-CRUD-%EC%98%88%EC%A0%9C?category=1008253)
  - [Spring Data JPA api](https://docs.spring.io/autorepo/docs/spring-data-commons/current/api/)
