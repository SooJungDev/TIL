## Step
- Step은 실제 Batch 작업을 수행하는 역할 
- Step 에서는 Batch로 실제 처리하고자 하는 기능과 설정을 모두 포함하는 장소 

## Next
~~~ java
   @Slf4j
   @Configuration
   @RequiredArgsConstructor
   public class StepNextJobConfiguration {
   
       private final JobBuilderFactory jobBuilderFactory;
       private final StepBuilderFactory stepBuilderFactory;
   
       @Bean
       public Job stepNestJob(){
           return jobBuilderFactory.get("stepNextJob")
                   .start(step1())
                   .next(step2())
                   .next(step3())
                   .build();
       }
   
       @Bean
       public Step step1(){
           return stepBuilderFactory.get("step1")
                   .tasklet((contribution, chunkContext) -> {
                       log.info(">>>> This is Step1");
                       return RepeatStatus.FINISHED;
                   }).build();
       }
   
       public Step step2(){
           return stepBuilderFactory.get("step2")
                   .tasklet((contribution, chunkContext) -> {
                       log.info(">>>> This is Step2");
                       return RepeatStatus.FINISHED;
                   }).build();
       }
   
       public Step step3(){
           return stepBuilderFactory.get("step3")
                   .tasklet((contribution, chunkContext) -> {
                       log.info(">>>> This is Step3");
                       return RepeatStatus.FINISHED;
                   }).build();
       }
   }
~~~
- next() 는 순차적으로 Step을 연결 시킬때 사용됨
- step1 -> step2 -> step3 순으로 하나씩 실행시킬때 next()는 좋은방법 

~~~
sprin.batch.job.names: ${job.name:NONE}
~~~
 - Spring Batch가 실행될때, Program argument로 job.name 값이 넘어오면 해당 값과 일치하는 job 만 실행하겠다
 -  ${job.name:NONE} :를 사이에 두고 좌측에 job.name이 있으면 값을 할당 없으면 NONE 을 할당하겠다는 의미
 - NONE 이 할당되면 어떤 배치도 실행하지 않겠다는 의미, 값이 없을떄 모든 배치가 실행되지 않도록 막음 
 
 ## 조건별 흐름제어 
 ~~~ java
 @Slf4j
 @Configuration
 @RequiredArgsConstructor
 public class StepNextConditionalJobConfiguration {
 
     private final JobBuilderFactory jobBuilderFactory;
     private final StepBuilderFactory stepBuilderFactory;
 
     @Bean
     public Job stepNextConditionalJob() {
         return jobBuilderFactory.get("stepNextConditionalJob")
                 .start(conditionalJobStep1())
                     .on("FAILED") //FAILED 일 경우
                     .to(conditionalJobStep3()) // step3으로 이동
                     .on("*") // step3의 결과 관계없이
                     .end()//step3으로 이동하면 flow 가 종료한다.
                 .from(conditionalJobStep1()) //step1
                     .on("*")// FAILED 외에 모든 경우
                     .to(conditionalJobStep2())// step2로 이동한다.
                     .next(conditionalJobStep3())// step2가 정상 종료되면 step3으로 이동한다
                     .on("*")// step3의 결과 관계없이
                     .end()//step3으로 이동하면 Flow가 종료한다.
                 .end()//Job 종료
                 .build();
     }
 
     @Bean
     public Step conditionalJobStep1() {
         return stepBuilderFactory.get("step1")
                 .tasklet((contribution, chunkContext) -> {
                     log.info(">>>> This is stepNextConditionalJob Step1");
 
                     /*
                      * ExitStatus 를 FAILED 로 지정한다.
                      * 해당 status를 보고 flow가 진행된다.
                      * */
 
                     contribution.setExitStatus(ExitStatus.FAILED);
                     return RepeatStatus.FINISHED;
                 })
                 .build();
     }
 
     @Bean
     public Step conditionalJobStep2() {
         return stepBuilderFactory.get("conditionalJobStep2").
                 tasklet((contribution, chunkContext) -> {
                     log.info(">>>>> This is stepNextConditionalJob Step2");
                     return RepeatStatus.FINISHED;
                 })
                 .build();
 
     }
 
     @Bean
     public Step conditionalJobStep3() {
         return stepBuilderFactory.get("conditionalJobStep3")
                 .tasklet((contribution, chunkContext) -> {
                     log.info(">>>>>> This is stepNextConditionalJob Step3");
                     return RepeatStatus.FINISHED;
                 }).build();
     }
 }
~~~
 - step1 실패 시나리오: step1 -> step3
 - step1 성공 시나리오: step1 -> step2 -> step3
 
 - .on()
     - 캐치할 ExitStatus 지정
     - * 일 경우 모든 ExitStatus가 지정됨
 - to()
    - 다음으로 이동할 Step 지정
 - from()
    - 일종의 이벤트 리스너 역할
    - 상태값을 보고 일치하는 상태라면 to()에 포함된 step을 호출함
    - step1의 이베트 캐치가 FAILED로 되어있는 상태에서 추가로 이벤트 캐치하려면 from 을 써야만함  
 - end()
    - end는 FlowBuilder를 반환하는 end와 FlowBuilder를 종료하는 end 2개가 있음
    - on("*") 뒤에 있는 end 는 FlowBuilder를 반환하는 end
    - build() 앞에 있는 end는 FlowBuilder를 종료하는 end
    - FlowBuilder 를 반환하는 end 사용시 계속해서 from 을 이어 갈 수 있음 
 
 - 여기서 중요한 점 on이 캐치하는 상태 값이 BatchStatus가 아닌 ExitStatus라는 점 
 - 분기처리를 위해 상태값 조정이 필요하다면 ExitStatus를 조정해야함
 
 ~~~
 .start(step1())
    .on("FAILED")
    .end()
 .from(step1())
    .on("COMPLETED WITH SKIPS")
    .to(errorPrint1())
    .end()
 .from(step1())
    .on("*")
    .to(step2())
    .end()  
 ~~~
  - step1이 실패하며, Job 또한 실패하게 된다.
  - step1이 성공적으로 완료되며, COMPLETED WITH SKIPS의 exit 코드로 종료됨
  - step1이 성공적으로 수행되어 step2가 수행됨
  
  위 코드에 나오는 COMPLETED WITH SKIPS exitCode를 반환하는 별도의 로직이 필요

~~~ java
 public class SkipCheckingListener extends StepExecutionListenerSupport{
    
    public ExitStatus afterStep(StepExecution stepExecution){
        String exitCode = stepExecution.getExitStatus().getExitCode();
        if(!exitCode.equals(ExitStatus.FAILED.getExitCode() &&
           stepExecution.getSkipCount() > 0 ){
                return new ExitStatus("COMPLETED WITH SKIPS");
           }
        else{
            return null;
        }   
    }
 }
~~~  
 - 위 코드를 설명하면 StepExecutionListener 에서는 먼저 Step이 성공적으로 수행되었는지 확인하고, StepExecution의 skip 횟수가
 0보다클경우 COMPLETED WITH SKIPS 의 exitCode를 갖는 ExitStatus를 반환함
 
## Decide
- 위에서 진행 했던 방식은 2가지 문제가 있음 
  - step이 담당하는 역할이 2개이상이 됨
    - 실제 Step이 처리해야할 로직외에도 분기처리를 시키기 위해 ExitStatus 조작이 필요함
  - 다양한 분기 로직 처리의 어려움
    - ExitStatus를 커스텀하게 고치기 위해선 Listener를 생성하고 Job Flow등록하는 등 번거로움 존재 
 
 ~~~
 @Slf4j
 @Configuration
 @RequiredArgsConstructor
 public class DeciderJobConfiguration {
     private final JobBuilderFactory jobBuilderFactory;
     private final StepBuilderFactory stepBuilderFactory;
 
     @Bean
     public Job deciderJob() {
         return jobBuilderFactory.get("deciderJob")
                 .start(startStep())
                 .next(decider())// 홀수 | 짝수 구분
                 .from(decider()) // decider의 상태가
                 .on("ODD") //ODD라면
                 .to(oddStep())//oddStep 으로간다.
                 .from(decider())// decider 상태가
                 .on("EVEN") //Even라면
                 .to(evenStep())//evenStep으로 간다.
                 .end()// builder 종료
                 .build();
 
     }
 
     @Bean
     public Step startStep() {
         return stepBuilderFactory.get("startStep")
                 .tasklet((contribution, chunkContext) -> {
                     log.info(">>>>> Start!!");
                     return RepeatStatus.FINISHED;
                 })
                 .build();
     }
 
     @Bean
     public Step evenStep() {
         return stepBuilderFactory.get("evenStep")
                 .tasklet((contribution, chunkContext) -> {
                     log.info(">>>>>> 짝수입니다");
                     return RepeatStatus.FINISHED;
                 }).build();
     }
 
     @Bean
     public Step oddStep() {
         return stepBuilderFactory.get("oddStep")
                 .tasklet((contribution, chunkContext) -> {
                     log.info(">>>>> 홀수입니다");
                     return RepeatStatus.FINISHED;
                 }).build();
     }
 
     @Bean
     public JobExecutionDecider decider() {
         return new OddDecider();
     }
 
     public static class OddDecider implements JobExecutionDecider {
 
 
         @Override
         public FlowExecutionStatus decide(JobExecution jobExecution, StepExecution stepExecution) {
             Random random = new Random();
 
             int randomNumber = random.nextInt(50) + 1;
             log.info("랜덤숫자:{}", randomNumber);
 
             if (randomNumber % 2 == 0) {
                 return new FlowExecutionStatus("EVEN");
             } else {
                 return new FlowExecutionStatus("ODD");
             }
 
 
         }
     }
 }
 ~~~    
  - startStep  -> oddDecider에서 홀수인지 짝수인지 구분 -> oddStep or evenStep 진행
   
## 참고사이트
  - [Spring Batch 가이드 - Spring Batch Job Flow ](https://jojoldu.tistory.com/328?category=635883)