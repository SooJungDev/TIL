## JobParameter와 Scope
~~~
@Value("#{jobParameters[파라미터명]}")
~~~
- @JobScope는 Step 선언문에서 사용 가능
- @StepScope Tasklet 이나 ItemReader, ItemWriter, ItemProcessor에서 사용
- Job Parameter의 타입으로 사용 가능 한 것 Double, Long, Date, String 등이 있음

## @StepScope & @JobScope 소개
~~~ java
@Bean
@StepScope
public ListItemReader<Integer> simpleWriterReader(){
    List<Integer> items = new ArrayList<>();
    
    for(int i=0; i<100; i++){
        items.add(i);
    }
    return new ListItemReader<>(items);
}
~~~
- Step 실행시점에 해당 컴포넌트를 Spring Bean으로 생성 
- @JobScope는 Job 실행 시점에 Bean이 생성
- Bean의 생성 시점을 지정된 Scope가 실행되는 시점으로 지연시킴
- JobParameter Late Binding이 가능 

## job paramter 의 오해
- Scope Bean을 생성할때만 가능
- @StepScope, @JobScope Bean을 생성할때만 Job Parameters 생성되기 때문에 사용
- **Job Parameters를 사용하기 위해선 꼭 @StepScope, @JobScope로 Bean을 생성!!**

## JobParameter vs 시스템 변수
- JobParameter
~~~ java
@Bean
@StepScope
public FlatFileItemReader<Partner> reader(
  @Value("#{jobParameters[pathToFile}") String pathToFile){
  FlatFileItemReader<Partner> itemReader = new FlatFileItemReader<Partner>();
  itemReader.setLineMapper(lineMapper());
  itemReader.setResource(new ClassPathResource(pathToFile));
  return itemReader;
}
~~~

- 시스템 변수 : application.properties와 -D 옵션으로 실행하는 변수까지 포함

~~~java
@Bean
@ConfigurationProperties(prefix ="my.prefix")
protected class JobProperties{
    String pathToFile;
    
    ...getters/ setters
}

@Autowired
private JobProperties jobProperties;

@Bean
public FlatFileItemReader<Partner> reader(){
    FlatFileItemReader<Partner> itemReader = new FlatFileItemReader<Partner>();
    itemReader.setLineMapper(lineMapper());
    String pathToFile = jobProperties.getPathToFile();
    itemReader.setResource(new ClassPathResource(pathToFile));
    return itemReader;
}
~~~
시스템 변수 사용시 문제
- Spring Batch의 Job Parameter 관련 기능을 못쓰게됨
- Job Parameter로 같은 Job을 두번 실행하지 않음
- Late Binding 못하게됨

~~~ java
@Slf4j
@RequiredArgsConstructor
@RestController
public class JobLauncerController {
    private final JobLauncher jobLauncher;
    private final Job job;
    
    public String handle(@RequestParam("fileName") String fileName) throws Exception{
        try{
          JobParameter jobParamters = new JobParamtersBuilder()
                                            .addString("input.file.name", fileName)
                                            .addLong("time",System.currentTimeMilis())
                                            .toJobParameters();
           jobLauncher.run(job, jobParameters);                                 
        }catch(Exception){
            log.info(e.getMessage());
        }
        
        return "Done";
    }
}
~~~
예제를 보면 Controller에서 Request Parameter로 받은 Job Parameter로 생성

~~~java
      JobParameter jobParamters = new JobParamtersBuilder()
                                            .addString("input.file.name", fileName)
                                            .addLong("time",System.currentTimeMilis())
                                            .toJobParameters();
~~~

그리고 생성한 Job PAramter로 Job을 수행함

~~~java
  jobLauncher.run(job, jobParameters);     
~~~
개발자가 원하는 어느 타이밍이든 Job Paramter를 생성하고 job을 수행 할 수 있음  
Job Paramter를 각각의 Batch 컴포넌트들이사용하면 변경이 심한 경우에도 쉽게 대응   
- 웹서버에서 Batch 관리하는것 권장하지 않음 



## 참고사이트
  - [Spring Batch 가이드 - Spring Batch Scope & Job Parameter](https://jojoldu.tistory.com/330?category=635883)
  - [@StepScope 사용시 주의사항]https://jojoldu.tistory.com/132)
