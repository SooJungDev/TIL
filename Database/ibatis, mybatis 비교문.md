## ibatis 비교문, mybatis 비교문 지원태그
### ibatis 비교문
- isNull : 널일경우
- isNotNull : 널이아닐경우
- isEmpty : 공백일경우
- isNotEmpty : 공백이아닐경우
- isGreaterTan : >
- isGreaterEqual : >=
- isLessThan : <
- isLessEqual : <=
- isEqual : ==
- isNotEqual : !=


### mybatis 비교문 지원 태그
- if: 단일 조건문
- choose when otherwise: 다중조건

- null 비교문
~~~
<!--ibatis -->
<isNull property="stringProperty">
    조건문
</isNull>
<!--mybatis-->
<if test="stringProperty == null">
    조건문
</if>

~~~




## 참고사이트
- [mybatis와 ibatis별 동적태그 비교문](https://hellogk.tistory.com/100)