## Mysql insert시 한글 입력 안될때
~~~ 
 Incorrect string value: '\xEC\x8B\xA4\xEB\xAA\x85...' 
~~~
- 이런메세지가 출력되면서 입력이안됨 
두가지 방법으로 해결할수있음  
- my.cnf 파일 설정변경
- Database, table 설정 변경해주기

일단 Database 자체에 디폴트 설정을 변경해주는것으로 해결함 
~~~
CREATE DATABASE 데이터베이스이름 default character set utf8
ALTER DATABASE  데이터베이스이름 character set utf8
CREATE TABLE 테이블이름 character set utf8
ALTER TABLE 테이블이름 character set utf8
~~~


## 참고사이트
  - [MySQL charset encoding](http://kwonnam.pe.kr/wiki/database/mysql/charset)
