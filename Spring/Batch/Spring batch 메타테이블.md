## BATCH_JOB_INSTANCE 테이블
- JOB_INSTANCE_ID
  - BATCH_JOB_INSTANCE 테이블의 PK
- JOB_NAME
    - 수행한 Batch Job Name
    
- **Job Parameter에 따라 생성되는 테이블**  
- Spring Batch가 실행될때 외부에서 받을 수 있는 파라미터 

- 특정 날짜를 Job Parameter 로 넘기면 Spring Batch에서 특정 날짜 데이터로 조회/가공/입력 등의 작업
- 같은 Batch Job이라도 Job Parameter 가 다르면 Batch_JOB_INSTANCE 에는 기록되며, Job Parameter가 같다면 기록되지 않음
 
- 동일한 Job이 Job Parameter 가 달라지면 그떄마다 BATCH_JOB_INSTANCE 에 생성됨
- 동일한 Job Parameter는 여러개 존재 할 수 없음 

## BATCH_JOB_EXECUTION

- JOB_EXECUTION과 JOB_INSTANCE는 부모-자식 관계
- JOB_EXECUTION은 자신의 부모 JOB_INSTANCE 가 성공/실패 했던 모든 내역을 갖고있음 
- 동일한 Job Parameter로 성공한 기록이 있을때만 재수행이 안됨 

## JOB, JOB_INSTANCE, JOB_EXECUTION
- job : 여기서는 simpleJob
- Job Instance: Job Parameter를 2018.08.01로 실행한 simpleJob(Job parameter 단위로 생성)
- Job Execution: Job Paramter 를 2018.08.01로 실행한 simpleJob의 1번째 시도 혹은 다음번 시도

## BATCH_JOB_EXECUTION_PARAM
- 


## 참고사이트
  - [3.Spring Batch 가이드 - 메타테이블엿보기](https://jojoldu.tistory.com/326?category=635883)
  - [Meta-Data Schema](https://docs.spring.io/spring-batch/3.0.x/reference/html/metaDataSchema.html)
