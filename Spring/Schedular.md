
## 스케줄러
- 스케쥴러, 잡스케줄러, 예약작업: 특정 시간/주기에 어떤일을 하는 예약 작업(job)들을 실행시켜주는 주체

## Quartz
- Quartz Scheduler 거의 모든 Java 응용 프로그램에 통합 할 수 있는 풍부한 기능의 오픈 소스 작업 스케줄 라이브러리
- Schedule 에 대한 clustering 기능 제공 
  - Database 기반으로 Scheduler key와 Trigger key를 통해 Schedule을 Clustering을 함
  - Quartz는 Database 기반으로 Schedule에 대한 제어가 가능하다는 말
  - job들을 적절히 분배하 실행 할 수 있으므로 효율적인 Batch 수행이 가능 할 것
- Scheduling을 위해 최적화된 구조를 제공 
 - Scheduler: 스케줄러와 상호 작용하기 위한 주요 API
 - Trigger: 주어진 작업이 싱핼될 일정을 정의하는 구성요소
 - JobDetail: 작업의 인스턴스를 정의하는데 사용




## 참고사이트
  - [Job Scheduler 크론 (Cron) 과 크론 표현식](https://gs.saro.me/dev?tn=548)
  - [Quartz + Spring Batch 조합하기](https://blog.kingbbode.com/posts/spring-batch-quartz)
  - [Quartz Spring Batch Example](https://examples.javacodegeeks.com/enterprise-java/spring/batch/quartz-spring-batch-example/)
