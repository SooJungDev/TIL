## trigger 란?
- 데이터베이스 트리거는 테이블에 대한 이벤트에 반응해 자동으로 실행되는 작업을 의미
- 트리거는 데이터 조작 언어 DML의 데이터 상태의 관리를 자동화하는데 사용된다.
- 트리거를 사용하여 데이터 작업 제한, 작업 기로그 변경 작업 감시 등을 할수 있다.
- **트리거란 테이블에 대한 이벤트가 발생했을때 자동으로 실행되는 작업** 

## 트리거 생성 구문
~~~
CREATE
    [DEFINER = user]
    TRIGGER trigger_name
    trigger_time trigger_event
    ON tbl_name FOR EACH ROW
    [trigger_order]
    trigger_body

trigger_time: { BEFORE | AFTER }

trigger_event: { INSERT | UPDATE | DELETE }

trigger_order: { FOLLOWS | PRECEDES } other_trigger_name
~~~

## trigger 목록 조회
- 해당 테이블이 다른 테이블에 trigger 걸려있는지 확인하는 쿼리
~~~
select *
from information_schema.triggers where ACTION_STATEMENT like %테이블이름%
~~~

## 참고사이트
- [TRIGGER(트리거) 및 DELIMITER(델리미터) 개념과 사용법](https://doorbw.tistory.com/23)
- [CREATE TRIGGER Syntax](https://dev.mysql.com/doc/refman/8.0/en/create-trigger.html)
- [mysql trigger 목록 조회](https://stackoverflow.com/questions/47363/how-do-you-list-all-triggers-in-a-mysql-database)