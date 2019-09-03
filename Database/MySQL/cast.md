## cast 
- mysql 은 비교나 검색을 수행할때 데이터 타입이 서로 다를 경우 내부적으로 타입이 같아지도록 자동변환하여 처리
- 하지만 사용자가 명시적으로 타입을 변환할수 있도록 다양한 연산자와  함수 제공
    - BINARY
    - CAST()
    - CONVERT()
    
- cast() 함수는 인수로 전달받은 값을 명시적 타입으로 변환하여 반환함
- 이때 변환하고자 하는 타입을 AS 절을 이용하여 직접 명시 할 수 있음

- 사용 방법
~~~
CAST(expr AS type)
~~~    

- 예제
~~~
CAST(DATE_FORMAT(CURRENT_TIME, '%H%i%s')AS signed int)
~~~
    
    
    
## 참고사이트
- [타입 변환](http://tcpschool.com/mysql/mysql_operator_typeCasting)    
    
    
  