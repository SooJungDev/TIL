## H2 데이터베이스 설치와 실행
- 최고의 실습용 DB
- 가볍다(1.5M)
- 웹용 쿼리툴 제공
- MySQL , Oracle 데이터베이스 시뮬레이션 기능
- 시퀀스, AUTO INCREMENT 기능 지원

## 메이븐 소개
- 자바 라이브러리, 빌드 관리
- 라이브러리 자동 다운로드 및 의존성관리
- 라이브러리 자동 다운로드 및 의존성 관리
- 최근에는 그래들(Gradle)이 점점 유명

## JPA 설정하기 - persistence.xml
- JPA 설정 파일
- /META-INF/persistence.xml 위치
- persistence-unit name으로 이름 지정
- javax.persistence 로 시작: JPA 표준 속성
- hibernate 로 시작: 하이버네이트 전용 속성

## 데이터베이스 방언
- JPA는 **특정 데이터베이스에 종속 X**
- 각각의 데이터베이스가 제공하는 SQL 문법과 함수는 조금씩 다름
 - 가변 문자 : MySQL 은 VARCHAR, Oracle 는 VARCHAR2
 - 문자열을 자르는 함수: SQL 표준은 SUBSTRING(), Oracle 은 SUBSTR()
 - 페이징 : MySQL 은 LIMIT , Oracle 은 ROWNUM
- 방언 : SQL 표준을 지키지 않는 특정 데이터베이스만의 고유한 기능

## 데이터베이스 방언
- hibernate.dialect 속성에 지정
 - H2: org.hibernate.dialect.H2Dialect
 - Oracle 10g: org.hibernate.dialect.Oracle10gDialect
 - MySQL : org.hibernate.dialect.MySQL5InnoDBDialect
- 하이버네이트는 40가지 이상의 데이터베이스 방언 지원

## JPA 구동방식
Persistence -> 1.설정 정보 조회 -> META-INF/persistence.xml
Persistence -> 2. 생성 -> EntityManagerFactory -> 3.생성 -> EntityManager


## 객체와 테이블을 생성하고 매핑하기
- @Entity: JPA 가 관리할 객체
- @Id : 데이터베이스 PK와 매핑
~~~java
@Entity
public class Member {
    @Id
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

~~~

## 주의
- **엔티티 매니저 팩토리**는 하나만 생성해서 애플리케이션 전체에서 공유
- **엔티티 매니저**는 쓰레드간에 공유X(사용하고 버려야 한다.)
- **JPA의 모든 데이터 변경은 트랜잭션 안에서 실행**

~~~java
public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();
        EntityTransaction tx = em.getTransaction();
        tx.begin();

        try {
            Member member = new Member();
            member.setId(2L);
            member.setName("helloB");
            em.persist(member);
            tx.commit();
        } catch (Exception e) {
            tx.rollback(); // 롤백
        } finally {
            em.close(); //무조건 닫아줘야 됨
        }
        emf.close();
    }
}
~~~

## JPQL 소개
- 가장 단순한 조회 방법
 - EntityManager.find()
 - 객체 그래프 탐색(a.getB().getC())
- 나이가 18살 이상인 회원을 모두 검색하고 싶다면?

## JPQL
- JPA를 사용하면 엔티티 객체를 중심으로 개발
- 문제를 검색쿼리
- 검색을 할 떄도 테이블이 아닌 엔티티 객체를 대상으로 검색
- 모든 DB 데이터를 객체로 변환해서 검색하는것은 불가능
- 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL 이 필요
- JPA 는 SQL을 추상화한 JPQL 이라는 객체 지향 쿼리 언어 제공
- SQL과 문법과 유사, SELECT , FROM, WHERE, GROUP BY, HAVING, JOIN 지원
- **JPQL은 엔티티 객체**를 대상으로 쿼리
- **SQL은 데이터베이스 테이블**을 대상으로 쿼리
- 테이블이 아닌 **객체를 대상으로 검색하는 객체 지향 쿼리**
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존X
- JPQL을 한마디로 정의하면 객체 지향 SQL
- **JPQL은 뒤에서 아주 자세히 다름**
