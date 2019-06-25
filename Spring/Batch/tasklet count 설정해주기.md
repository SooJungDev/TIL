## tasklet write, read count 설정해주기

- chunkContext.getStepContext().getStepExecution() 을 받아와서 set 해주었음

~~~java

public RepeatStatus execute(StepContribution contribution, ChunkContext chunkContext) throws Exception {
    int items = dao.deleteItems();
    chunkContext.getStepContext().getStepExecution().setReadCount(items);
    chunkContext.getStepContext().getStepExecution().setWriteCount(items);
    return RepeatStatus.FINISHED;
}
~~~

## 참고사이트 
- [How to count items in a Tasket in spring-batch?](https://stackoverflow.com/questions/35222681/how-to-count-items-in-a-tasket-in-spring-batch)