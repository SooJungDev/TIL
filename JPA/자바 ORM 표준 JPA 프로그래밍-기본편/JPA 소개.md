## SQL 중심적인 개발의 문제점
애플리케이션
객체 지향 언어 java, scala ....
데이터베이스 세계 헤게모니 관계형 DB - oracle, mysql
지금 시대는 객체를 관계형 DB에 관리

SQL 중심적인 개발의 문제점
- 무한반복, 지루한코드
- SQL에 의존적인 개발을 피하기 어렵다

패더다임의 불일치
- 객체 vs 관계형 데이터베이스
- 객체 지향 프로그래밍 추상화,캡슐화, 정보은닉,상속, 다형성들 시스템 복잡성을 제어할수있는 다양한 장치들을 제공한다

객체를 영구 보관하는 다양한 저장소
- RDB
- NoSQL
- File
- OODB?

현실적인 대안은 관계형 데이터베이스

객체와 관계형 데이터베이스의 차이
1. 상속
    - DB 에 저장할 객체에는 상속 관계 안쓴다.
2. 연관관계
    - 객체는 참조를 사용
    - 테이블은 외래키를 사용
3. 데이터 타입
4. 데이터 식별 방법

SQL 직접 다루게 되면
계층형 아키텍처 진정한 의미의 계층분할이 어렵다.

- 객체답게 모델링 할수록 매핑 작업만 늘어난다.
- 객체를 자바 컬렉션 저장하듯이 DB에 저장할 수 없을까?

## JPA
- Java Persistence API
- 자바 진영의 ORM 기술 표준

### ORM
- Object relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재

### JPA 는 애플리케이션과 JDBC 사이에서 동작
JAVA 애플리케이션 JPA -> JDBC API -> SQL ->DB
- 결과 반환
- **패러다임 불일치 해결**

### JPA 소개
             하이버네이트 (오픈소스)
------------------------------------------->
EJB - 엔티티 빈(자바 표준)           JPA(자바표준)

### JPA는 표준명세
- JPA는 인터페이스의 모음
- JPA 2.1 표준명세를 구현한 3가지 구현체
- 하이버네이트 , EclipseLink, DataNucleus


## JPA를 왜 사용해야 하는가?
- SQL 중심적인 개발에서 객체 중심으로 개발
- 생산성
- 유지보수
- 패러다임의 불일치 해결
- 성능
- 데이터 접근 추상화에 벤더 독립성
- 표준

## 생산성
- 저장 : jpa.persist(member)
- 조회 : Member member = jpa.find(memberId)
- 수정 : member.setName("변경할 이름")
- 삭제: jpa.remove(member)

## 유지보수
- JPA 필드만 추가하면됨 SQL 은 JPA가 처리

## JPA 와 패러다임의 불일치 해결
1. JPA와 상속
2. JPA와 연관관계
3. JPA와 객체 그래프 탐색
4. JPA와 비교하기
 - 동일한 트랜잭션에서 조회한 엔티티는 같음을 보장

### JPA의 성능 최적화 기능
1. 1차 캐시와 동일성(identity)보장
2. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
3. 지연 로딩(Lazy Loading)

### 1차 캐시와 동일성 보장
1. 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
2. DB Isolation Level이 Read Commit 이어도 애플리케이션 Repeatable Read 보장

~~~java
String memberId = "100";
Member m1 = jpa.find(Member.class, memberId); //SQL
Member m2 = jpa.find(Member.class, memberId); //캐시
println(m1 == m2) //true
~~~
- SQL 1번만 실행

### 트랜잭션을 지원하는 쓰기 지연 - INSERT
1. 트랜잭션을 커밋할때까지 INSERT SQL을 모음
2. JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송
~~~java
transaction.begin(); // [트랜잭션] 시작
 
em.persist(memberA); em.persist(memberB); em.persist(memberC);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.
//커밋하는 순간 데이터베이스에 INSERT SQL을 모아서 보낸다.
transaction.commit(); // [트랜잭션] 커밋
~~~

### 트랜잭션을 지원하는 쓰기지연 - UPDATE
1. UPDATE, DELETE로 인한 로우(ROW)락 시간 최소화
2. 트랜잭션 커밋 시 UPDATE, DELETE SQL 실행하고, 바로 커밋
~~~java
transaction.begin(); // [트랜잭션] 시작
 
changeMember(memberA);
deleteMember(memberB);
비즈니스_로직_수행(); //비즈니스 로직 수행 동안 DB 로우 락이 걸리지 않는다.
//커밋하는 순간 데이터베이스에 UPDATE, DELETE SQL을 보낸다.
transaction.commit(); // [트랜잭션] 커밋
~~~

### 지연 로딩과 즉시로딩
- 지연로딩: 객체가 실제 사용될때 로딩
- 즉시로딩: JOIN SQL로 한번에 연관된 객체까지 미리조회

**ORM 은 객체와 RDB 두 기둥위에 있는 기술**

