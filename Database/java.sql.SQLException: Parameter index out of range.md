## java.sql.SQLException: Parameter index out of range
- 아래와같은 에러가 나타났음 ibatis 에서

~~~
ibatis Cause: java.sql.SQLException: Parameter index out of range (2 > number of parameters, which is 1).
~~~

- 해당쿼리는 db툴에서 잘 동작했는데 실제로 호출될때 되지 않았음
- 나같은 경우에는 # 으로된 .. 주석이있었고 -- 으로된 주석도있었는데 다삭제해줌 쿼리안에는 주석안넣는것이 좋을꺼같음!
- 없애주니 잘동작하였다.