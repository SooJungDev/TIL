## group_concat
- in 구문에 넣을 인자를 만들어야 되는 일이 생김 
- group_concat 을 사용하면 , 로 원하는 값을 한줄로 만들 수있음


~~~
select group_concat(필드명) from 테이블 명 limit 100;
~~~
- 기본 형태로 사용 했을 경우 문자열 사이에 쉼표가 붙는다

다양하게 사용 할 수 있음 아래와 같이 해준다.
1. 기본형 : group_concat(필드명)
2. 구분자 변경 : group_concat(필드명 separator '구분자')
3. 중복제거 : group_concat(distinct 필드명)
4. 문자열 정렬 : group_concat(필드명 order by 필드명)



## 참고사이트
- [GROUP_CONCAT 사용하기](https://fruitdev.tistory.com/16)