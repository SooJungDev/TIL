## 인메모리 데이터베이스 지원
지원하는 인-메모리 데이터베이스
- H2(추천, 콘솔때문에)
- HSQL
- Derby

Spring-JDBC 가 클래스패스에 있으면 자동 설정이 필요한 빈을 설정해줍니다.
- DataSource
- JdbcTemplate

인-메모리 데이터베이스 기본 연결 정복 확인하는 방법
- URL: "testdb"
- username "sa"
- password: ""

H2콘솔 사용하는방법
- h2-console의 초기 JDBC URL 설정이 jdbc:h2:~/test로 되어 있는데, **jdbc:h2:mem:testdb** 로 변경해줘야 콘솔에서 정상으로 보임
- spring-boot-devtools를 추가하거나
- spring.h2.console.enabled= true 만 추가.
- /h2-console 로 접속 (이 path도 바꿀수 있음)

실습코드
- CREATE TABLE USER(ID INTEGER NOT NULL, NAME VARCHAR(255), PRIMARY KEY(ID))
- INSERT INTO USER VALUES(1,"soojung")

## Mysql
지원하는 DBCP (Data Base Connection Pool)
- [HikariCP 기본](https://github.com/brettwooldridge/HikariCP)
 - 커넥션 객체 기본적으로 10개
- [Tomcat CP](https://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html)
- [Commnos DBCP2](https://commons.apache.org/proper/commons-dbcp/)

DBCP 설정
- spring.datasource.hikari.*
- spring.datasource.tomcat.*
- spring.datasource.dbcp2.*

MySQL 커넥터 의존성 추가
~~~
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
</dependency>
~~~

MySQL 추가(도커 사용)
- docker run -p 3306:3006 --name mysql_boot -e MYSQL_ROOT_PASSWORD=1 -e MYSQL_DATABASE=springboot -e MYSQL_USER=soojung -e MYSQL_PASSWORD=pass -d mysql
- docker exec -i t mysql_boot bash
- mysql -u soojung -p
- docker inspect mysql_boot | grep IP 
  => localhost 로 안되서 해당 mysql 에 ip 주소로 했더니 잘됨 ㅠㅠ

MySQL 용 Datasource 설정
- spring.datasource.url=jdbc:mysql://localhost:3306:/springboot?useSSL=false
- spring.datasource.username = soojung
- spring.datasource.password = pass


MYSQL 접속시 에러
 강좌로 돌아가기 백기선의 프로필 사진강사 
백기선  15 분
스프링 데이터 3부: MySQL

지원하는 DBCP
HikariCP (기본)
https://github.com/brettwooldridge/HikariCP#frequently-used
Tomcat CP
Commons DBCP2
DBCP 설정
spring.datasource.hikari.*
spring.datasource.tomcat.*
spring.datasource.dbcp2.*
MySQL 커넥터 의존성 추가
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
</dependency>
MySQL 추가 (도커 사용)
docker run -p 3306:3306 --name mysql_boot -e MYSQL_ROOT_PASSWORD=1 -e MYSQL_DATABASE=springboot -e MYSQL_USER=soojung -e MYSQL_PASSWORD=pass -d mysql
docker exec -i -t mysql_boot bash
mysql -u root -p

MySQL용 Datasource 설정
spring.datasource.url=jdbc:mysql://localhost:3306/springboot?useSSL=false
spring.datasource.username=keesun
spring.datasource.password=pass

MySQL 접속시 에러

MySQL 5.* 최신 버전 사용할 때

| 문제 | Sat Jul 21 11:17:59 PDT 2018 WARN: Establishing SSL connection without server’s identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn’t set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to ‘false’. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification. |
|-----:|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 해결 | jdbc:mysql:/localhost:3306/springboot?useSSL=false |


MySQL 8.* 최신 버전 사용할 때


| 문제 | com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Public Key Retrieval is not allowed |
|-----:|-----------------------------------------------------------------------------------------------------------|
| 해결 | jdbc:mysql:/localhost:3306/springboot?useSSL=false&allowPublicKeyRetrieval=true |


MySQL 라이센스 (GPL) 주의
- MySQL 대신 MariaDB 사용검토
- 소스코드 공개 의무 여부 확인

## PostgreSQL
의존성 추가
~~~
<dependency>
   <groupId>org.postgresql</groupId>
   <artifactId>postgresql</artifactId>
</dependency>
PostgreSQL 설
~~~

PostgreSQL 설치 및 서버 실행(docker)
~~~
docker run -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=keesun -e POSTGRES_DB=springboot --name postgres_boot -d postgres

docker exec -i -t postgres_boot bash

su - postgres

psql -U keesun springboot

데이터베이스 조회
\list

테이블 조회
\dt

쿼리
SELECT * FROM account;
~~~

PostgreSQL 경고 메세지
경고: org.postgresql.jdbc.PgConnection.createClob() is not yet implemented
해결: spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true

## 스프링 데이터 jpa
ORM(Object-Relational Mapping)과 JPA(Java Persistence API)
- 객체와 릴레이션을 맵핑할때 발생하는 개념적 불일치를 해결하는 프레임워크
- JPA: ORM을 위한 자바 표준
스프링 데이터 JPA 의존성추가
~~~
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
~~~
스프링 데이터 JPA 사용하기
- @Entity 클래스 만들기
- Repository 만들기
- Spring Data JPA -> JPA-> Hibernate -> Datasource

스프링 데이터 리파지토리 테스트 만들기
- H2 DB를 테스트 의존성에 추가하기
- @DataJpaTest(슬라이스 테스트)작성

인텔리제이에서 테스트 작성할때 해당 클래스위에 command+shift+t 누르면 자동으로 셍상됨

## 데이터베이스 초기화
JPA를 사용한 데이터베이스 초기화
- spring.jpa.hibernate.ddl-auto=update
- spring.jpa.generate-ddl=true 설정해줘야 동작함
- spring.jpa.show-sql=true

운영일 경우
spring.jpa.hibernate.ddl-auto=validate => 검증만 해줌
spring.jpa.generate-ddl=false

SQL 스크립트를 사용한 데이터베이스 초기화
- schema.sql 또는 schema-${platform}.sql (순서 제일 처음)
- data.sql 또는 data-${platform}.sql (그다음)
- ${platform} 값은 spiring.datasource.platform으로 설정가능

## 데이터베이스 마이그레이션
Flyway와 Liquibase가 대표적인데 Flyway 사용
[Flyway reference](https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#howto-execute-flyway-database-migrations-on-startup)
의존성 추가
- org.flywaydb:flyway-core

마이그레이션 디렉토리
- db/migration 또는 db/migration/{vendor}
- spring.flyway.locations로 변경 가능

마이그레이션 파일 이름
- V숫자_이름.sql
- V는 꼭 대문자로.
- 숫자는 순차적으로 (타임스탬프 권장)
- 숫자와 이름사이에 언더바 두개
- 이름은 가능한 서술적으로 

## Redis
캐시 , 메시지브로커, 키/밸류 스토어 등으로 사용가능.

- 의존성 추가
 - spring-boot-starter-data-redis
 
- Redis 설치 및 실행(도커)
 - docker run -p 6379:6379 --name redis_boot -d redis
 - docker exec -i -t redis_boot redis-cli
 
- 스프링 데이터 Redis
 - [Spring Data Redis](http://spring.io/projects/spring-data-redis)
 - StringRedisTemplate 또는 RedisTemplate
 - extends CrudRepository
 
- [Redis 주요 커맨드](https://redis.io/commands)
 - keys *
 - get {key}
 - hgetall {key}
 - hget {key}{column}

- 커스터마이징
 - spring.redis.*

## MongoDB
- [MongoDB](https://www.mongodb.com/)는 JSON 기반의 도큐먼트 데이터 베이스 (스키마가 없는게 특징)

- 의존성 추가
 - docker run -p 27017:27017 -- name mongo_boot -d mongo
 - docker exec -i -t mongo_boot bash
 - mongo
- 스프링 데이터 몽고 DB
 - MongoTemplate
 - MongoRepository
 - 내장용 MongoDB (테스트용)
  - de.flapdoodle.embed:de.flapdoodle.embed.mongo
 - @DataMongoTest

## Neo4j
- [Neo4j](https://neo4j.com/)는 노드간의 연관관계를 영속화하는데 유리한 그래프 데이터베이스임

- 의존성추가
 - spring-boot-starter-data-neo4j
 
- Neo4j 설치 및 실행(도커)
 - docker run -p 7474:7474 -p 7687:7687 -d --name neo4j_boot neo4j
 - http://localhost:7474/browser
 
- 스프링 데이터 Neo4J
 - Neo4jTemplate (Deprecated)
 - SessionFactory
 - Neo4jRepository

## 정리
- [springboot sql reference](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-sql)
