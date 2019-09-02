## function

- 일단 db 에 있는 function 목록 불러오기
~~~~
show function status;
~~~~

- 만들어진걸 자세히 보고 싶은 function 조회하기
- 해당 이름을 넣은다음에 조회한다.
~~~
show create function 이름;
~~~  

- 호출시 
~~~
select 함수이름(인자);
~~~

## function 생성 예제
~~~
DELIMITER $$
 
CREATE FUNCTION CustomerLevel(p_creditLimit double) RETURNS VARCHAR(10)
    DETERMINISTIC
BEGIN
    DECLARE lvl varchar(10);
 
    IF p_creditLimit > 50000 THEN
        SET lvl = 'PLATINUM';
    ELSEIF (p_creditLimit <= 50000 AND p_creditLimit >= 10000) THEN
        SET lvl = 'GOLD';
    ELSEIF p_creditLimit < 10000 THEN
        SET lvl = 'SILVER';
    END IF;
 
 RETURN (lvl);
END
~~~

## 참고사이트
- [Stored Function](http://javapia.blogspot.com/2010/07/mysql-stored-procedurestored-function.html)
- [MySql Stored Function](https://m.blog.naver.com/PostView.nhn?blogId=sthwin&logNo=221150189755&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
