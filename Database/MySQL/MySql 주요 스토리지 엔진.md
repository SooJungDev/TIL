## 주요 스토리지 엔진(Storage Engine)간단비교

MySQL 크게 아래의 2가지 구조
- 서버 엔진 : 클라이언트(또는 사용자)가 Query를 요청했을때 Query Parsing 과 스토리지 엔진에 데이털르 요청하는 작업을 수행
- 스토리지 엔진: 물리적 저장장치에서 데이터를 읽어오는 역할을 담당

- 엔진 종류마다 동작원리가 다르고 따라서 트랜잭션, 성능과 같은 주요 이슈도 밀접하게 연관되어 있음
- MySQL **스토리지 엔진은 Plug in 방식이며, 기본적으로 8가지 스토리지 엔진**이 탑재되어있다.
- 아래의 명령으로도 탑재된 스토리지 엔진을 확인 할 수 있다.
~~~
SHOW ENGINES
~~~

- plug in 방식이기 떄문에 스토리지 엔진 교체 작업이 비교적 간단함
- [스토리지 엔진 설정하는법](https://dev.mysql.com/doc/refman/5.7/en/storage-engine-setting.html)

가장 많이 쓰이는 스토리지 엔진은 크게 3가지
**MyISAM, InnoDB , Archive 의 3가지 스토리지 엔진**

InnoDB
- 따로 스토리지 엔진을 명시하지 않으면 **default로 설정되는 스토리지 엔진이다.** InnoDB는 **transaction-safe** 하며
**커밋과 롤백, 그리고 데이터 복구 기능을 제공하므로 데이터를 효과적으로 보호** 할 수 있다.
- InnoDB 기본적으로 row-level locking 제공하며, 또한 데이터를 clustered index에 저장하여 PK 기반의 query I/O 비용을 줄인다
또한 FK 제약을 제공하여 데이터 무결성을 보장한다.

MyISAM
- 트랜잭션을 지원하지 않고 table-level-locking을 제공한다. 따라서 multi-thread 환경에서 성능이 저하 될수도있다.
- 특정 세션이 테이블을 변경하는 동안 테이블 단위로 lock 이 잡히기 떄문이다
- 텍스트 전문검색(Fulltext searching)과 지리정보 처리 기능도 지원됨
    - 이를 사용할시 파티셔닝이 되지않는 단점
 
Archive
- **로그 수집**에 적합한 엔진
- 데이터가 메모리 상에서 압축되고 압축된 상태로 디스크에 저장되기 떄문에 row-level locking 이 가능하다
- **다만 한번 insert 된 데이터는 update, delete 를 사용 할 수 없으며 인덱스를 지원하지 않는다.**
- 따라서 거의 가공하지 않을 원시 로그데이터를 관리하는데 효울적일 수 있고 테이블 파티셔닝도 지원한다.
    - 다만 트랜잭션은 지원하지 않는다



참고사이트 
- [Alternative Storage Engines](https://dev.mysql.com/doc/refman/5.7/en/storage-engines.html)
- [[MySQL] 주요 스토리지 엔진(Storage Engine) 간단 비교](http://asuraiv.blogspot.com/2017/07/mysql-storage-engine.html?m=1)
- [[MySQL] 적절한 스토리지엔진 선택](https://12bme.tistory.com/95)
