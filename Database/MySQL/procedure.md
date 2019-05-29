## 저장 프로시져란?
- 여러 SQL 문을 하나의 SQL 문처럼 정리하여 CALL 이라는 명령으로 실행 할 수 있게 만드는것을 저장프로시저라고함(Stored procedure)
    - Stored 는 저장하다 Procedure 은 절차 즉 일련의 절차를 정리해서 저장한것이다.
- 몇개의 명령문을 입력해도 상관없음 또한 변수를 사용하거나 IF나 CASE 로 분기 조건을 설정하거나, WHILE 이나 REPEAT 반복 처리할수도있음

## 저장 프로시저 작성하기
~~~
CREATE PROCEDURE 저장프로시저이름()
BEGIN
    SQL 문1
    SQL 문2
END 
~~~
- BEGIN에서 END 까지 저장 프로시저의 본체
- 명령범위를 명확히 하고있음
~~~
BEGIN
SELECT * FROM tb;
SELECT * FROM tb1;
END
~~~

- ; 저장 프로시저가 완성되지 않는 상태에서 CREATE PROCEDURE 명령이 실행되고 맘 mysql 콘솔창은 ; 입력되면 어떤경우에건 ; 이전 단계까지 명령문을 실행하게된다.

- 구분문자 // 변경하기
~~~
DELIMITER //
~~~


~~~
DELIMITER //

CREATE PROCEDURE pr1()

BEGIN

SELECT * FROM tb;

SELECT * FROM tb1;

END

//


DELIMITER ;
~~~
- **DELIMITER ; 마지막 ;  돌려놔야함!!!!!!!!!**

## 저장프로시저 실행하기
~~~
CALL 저장프로시저이름;
~~~

- 위에 pr1 호출하기
~~~
CALL pr1;
~~~


## 참고사이트
- [저장 프로시저 활용하기](https://recoveryman.tistory.com/186)