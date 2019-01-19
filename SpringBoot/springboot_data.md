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
지원하는 DBCP
- [HikariCP 기본](https://github.com/brettwooldridge/HikariCP)
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
- docker run -p 3306:3006 -name mysql_boot -e MYSQL_ROOT_PASSWORD=1 -e MYSQL_DATABASE=springboot
 -e MYSQL_USER=soojung -e MYSQL_PASSWORD=pass -d mysql
- docker exec -i t mysql_boot bash
- mysql -u root -p

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
docker run -p 3306:3306 –name mysql_boot -e MYSQL_ROOT_PASSWORD=1 -e MYSQL_DATABASE=springboot -e MYSQL_USER=keesun -e MYSQL_PASSWORD=pass -d mysql
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



