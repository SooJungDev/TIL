## INFORMATION_SCHEMA Tables

- 일단 로우수 조회시 count 로 할경우에 무리가 간다고 해서 아래와같이 사용함
- 해당테이블의 전체로우수 
~~~
select TABLE_ROWS from information_schema.tables where table_name = '테이블이름';
~~~


- 참고사이트 [INFORMATION_SCHEMA Tables](https://dev.mysql.com/doc/refman/8.0/en/information-schema.html)