## EXPLAIN 명령어
~~~
mysql> EXPLAIN SELECT ...₩G 
~~~
- select 문장에만 사용
- ₩G 를 사용하면 더 보기좋게 나옴 

## SELECT_TYPE
- SIMPLE , PRIMARY : 가장 바깥 쿼리
- SUBQUERY: 일반 서브쿼리
- DERIVED : FROM절의 서브쿼리 (가능한 없애는것이 좋음)
- DEPENDANT SUBQUERY : 바깥테이블과 연관된 서브쿼리(가능한 없애는것이 좋음)

## TABLE
테이블 이름 또는 종류 <xx2> 와 같은 테이블의 숫자는 쿼리의 플랜 id를 가리킴

## TYPE
- 실제 데이터를 읽는 방법
- SYSTEM,CONST, REF, RANGE, INDEX, ALL 등이있음
- SYSTEM 이 가장 빠르고 ALL 이 가장 느림
- INDEX는 INDEX FULL SCAN 빠르지 않음

## KEY
- 실제 데이터를 읽기 위해 사용되는 인덱스의 이름
- 필요에 의해 생성한 인덱스가 잘 사용되는지 확인

## KEY_LEN 
- 인덱스 중 사용 할 수 있는 크기를 나타냄
- 복합 인덱스에서 매우 중요합니다

## ROWS
- 인덱스 중 사용 할 수 있는 크기를 나타냄
- 복합 인덱스에서 매우중요

## ROWs
- 예상 레코드 개수, 이를 위해 통계정보를 저장

## 요약
1. 쿼리가 원하는 성능이 안나올 경우 튜닝실시
2. explain 명령어로 원인분석
3. dependatn subquery, derived와 같은 타입이안나오게
4. 되도록이면 all이 나오지 않도록

## 참고사이트
  - [기초 튜닝](https://www.slideshare.net/hoyoung2jung/ss-42982390)
  - [mysql EXPLAIN ](https://dev.mysql.com/doc/refman/5.5/en/explain-output.html)
