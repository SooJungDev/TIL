## Mysql 세가지 특성
- 단일 코어에서 Nested Loop join 처리
- 다양한 스토리지 엔진
- 데이터 복제(Replication) 기능

## Join 에 대해서
 join 종류는 5가지가 있음
 1. INNER JOIN
 2. OUTER JOIN
 3. CROSS JOIN
 4. FULL OUTER JOIN
 5. SELF JOIN

join의 방식은 3가지
- Nested Loop Join: 중첩반복
- Merge Join : 정렬병합
- Hash Join : 해시 매치

## 중첩반복(Nested Loops)조인
- 바깥 테이블의 처리 범위를 하나씩 엑세스 하면서 그 추출된 값으로 안쪽 테이블 조인하는 방식
1. 순차적으로 처리된다.
2. 바깥 테이블과 일치하는 값을 안쪽 테이블에서 찾아야 하므로 안쪽 테이블의 해당 열에 인덱스가 필요하다.
3. 메모리 사용량은 가장 적다.
4. 바깥 테이블과 안쪽 테이블의 크기는 성능과 관련이 없다. 


## 참고사이트
  - [Mysql 특성](http://gywn.net/2011/12/mysql-three-features/)
  - [Join 방식](https://blog.sonim1.com/108)
  