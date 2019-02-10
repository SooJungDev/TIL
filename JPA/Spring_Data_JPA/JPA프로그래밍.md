## 프로젝트 세팅

데이터베이스 실행
- Postgre SQL 도커 컨테이너 재사용
- docker start postgres_boot

스프링 부트
- 스프링 부트 v2.*
- 스프링 프레임워크 v5.*

스프링 부트 스타터 JPA
- JPA 프로그래밍에 필요한 의존성 추가
    - JPA v2.*
    - Hibernate v5.*
- 자동설정: HibernateJpaAutoConfiguration
    - 컨테이너가 관리하는 EntityManager(프록시) 빈설정
    - Platform TransactionManager 빈 설정

JDBC 설정
- jdbc:postgresql://localhost:5432/springdata
- soojung
- pass

application.properties
- spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true
- spring.jpa.hibernate.ddl-auto=create
    - 운영시 validate 로 사용하는것이 좋음
    - update 사용시 편할 수 있으나 스키마가 더러워질수있음 (컬럼이 빠지거나 이름변경시 알수없음 자동으로 지워지지않음)
    
## 엔티티 맵핑
@Entity
- 엔티티는 객체 세상에서 부르는 이름
- 보통 클래스와 같은 이름을 사용하기때문에 값을 변경하지 않음
- 언티티의 이름은 JQL에서 쓰임

@Table
- 릴레이션 세상에서 부르는 이름
- @Entity의 이름이 기본값
- 테이블의 이름은 SQL에서 쓰임

@Id
- 엔티티의 주키를 맵핑할때 사용
- 자바의 모든 primitive 타입과 그 랩퍼 타입을 사용 할 수 있음
    - Date랑 BigDecimal, BigInteger도 사용가능
- 복합키를 만드는 매핑하는 방법도 있지만 그건 논외로

@GeneratedValue
- 주키의 생성방법을 맵핑하는 애노테이션
- 생성 전략과 생서기를 설정 할 수있음
    - 기본전략은 AUTO: 사용하는 DB에 따라 적절한 전략 선택
    - TABLE, SEQUENCE, IDENTITY 중 하나

@Column
- unique
- nullable
- length
- columnDefinition
- ...

@Temporal
- 현재 JPA 2.1까지는 Date, Calendar만 지원

@Transient
- 컬럼으로 맵핑하고 싶지 않은 멤버 변수에 사용

application.properties
- spring.jpa.show-sql=true
- spring.jpa.properties.hibernate.format_sql=true
