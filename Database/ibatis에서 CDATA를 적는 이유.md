## ibatis에서 CDATA 적는 이유
- xml 문서 내 쿼리안에 <>& 등의 특수문자가 포함될 경우 에러 방지하기 위해서

~~~
   SELECT * FROM DUAL WHERE  A <![CDATA[ > ]]> B
~~~

- CDATA 를 사용하지 않고 ibatis에서 사용하려면 밑에와같이

~~~
   SELECT * FROM DUAL WHERE  A &gt; B
~~~

- 또한 <![CDATA[ SQL ]]> 내에서 다이나믹 쿼리는 쓰지 못함

## 참고사이트
- [iBATIS에서 CDATA를 적는 목적](https://kmj1107.tistory.com/entry/iBATIS%EC%97%90%EC%84%9C-CDATA%EB%A5%BC-%EC%A0%81%EB%8A%94-%EB%AA%A9%EC%A0%81)