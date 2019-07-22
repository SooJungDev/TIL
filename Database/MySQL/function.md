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

## 참고사이트
- [Stored Function](http://javapia.blogspot.com/2010/07/mysql-stored-procedurestored-function.html)