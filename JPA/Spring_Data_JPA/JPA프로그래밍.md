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
    
