## Chunk란?
- Spring Batch에서 Chunk란 데이터 덩어리로 작업 할대 각 커밋 사이에 처리되는 row 수를 이야기함 
- Chunk 지향 처리란 한번에 하나씩 데이터를 읽어 Chunk라는 덩어리를 만든뒤, Chunk 단위로 트랙잭션을 다루는것을 의미 
- Chunk 단위로 트랙잭션을 수행, 실패할 경우 해당 Chunk만큼만 롤백되고 이전에 커밋된 트랜잭선 범위 까지는 반영 

chunk 단위 처리
- Reader에서 데이터 하나를 읽어옴
- 읽어온 데이터를 Processor에서 가공
- 가공된 데이터들을 별도의 공간에 모은뒤, chunk 단위 만큼 쌓이게 되면 Writer에 전달하고 Writer는 일괄저장
- **Reader와 Processor에서는 1건씩 다뤄지고, Writer에선 Chunk 단위로 처리됨**

~~~ java
for(int i=0; i<totalSize; i+=chunckSize){ // chunkSize 단위로 묶어서 처리
    List items = new Arraylist();
    for(int j=0; j< chunkSize; j++){
        Object item = itemReader.read();
        Object processedItem = itemProcessor.process(item);
        items.add(ProcessedItem);
    }
    itemWriter.write(itmes);
}
~~~
**chunkSize 별로 묶어서 처리됨**

## commit-interval
- commit 이 되는 단위 개수
- 트랜잭션 커밋이 호출되기전에 처리되어야 하는 아이템들의 갯수
- chunk 메소드 chunkSize 인자값으로 입력되는 값이 commit-interval 값이다
- 이갯수는 reader 가 읽고 processor 가 처리해서 writer 로 넘겨지는 갯수를 의미
- 그리고 트랜잭션이 설정되어 있다면 이갯수로 단위로 트랜잭션 커밋이 발생한다.
- 스프링 배치는 reader 가 읽고나서 processor 처리한 갯수가 commit-interval 만큼 쌓이면 writer item들을 보내서 write 함
- 결국 위에 !!! 이야기랑 똑같음


## ChunkOrientedTasklet 
~~~ java

/**
 * A {@link Tasklet} implementing variations on read-process-write item
 * handling.
 *
 * @author Dave Syer
 *
 * @param <I> input item type
 */
public class ChunkOrientedTasklet<I> implements Tasklet {

	private static final String INPUTS_KEY = "INPUTS";

	private final ChunkProcessor<I> chunkProcessor;

	private final ChunkProvider<I> chunkProvider;

	private boolean buffering = true;

	private static Log logger = LogFactory.getLog(ChunkOrientedTasklet.class);

	public ChunkOrientedTasklet(ChunkProvider<I> chunkProvider, ChunkProcessor<I> chunkProcessor) {
		this.chunkProvider = chunkProvider;
		this.chunkProcessor = chunkProcessor;
	}

	/**
	 * Flag to indicate that items should be buffered once read. Defaults to
	 * true, which is appropriate for forward-only, non-transactional item
	 * readers. Main (or only) use case for setting this flag to false is a
	 * transactional JMS item reader.
	 *
	 * @param buffering indicator
	 */
	public void setBuffering(boolean buffering) {
		this.buffering = buffering;
	}

	@Override
	public RepeatStatus execute(StepContribution contribution, ChunkContext chunkContext) throws Exception {

		@SuppressWarnings("unchecked")
		Chunk<I> inputs = (Chunk<I>) chunkContext.getAttribute(INPUTS_KEY);
		if (inputs == null) {
			inputs = chunkProvider.provide(contribution); //Reader에서 데이터 가져옴
			if (buffering) {
				chunkContext.setAttribute(INPUTS_KEY, inputs);
			}
		}

		chunkProcessor.process(contribution, inputs); //Processor & Writer 처리
		chunkProvider.postProcess(contribution, inputs); 

		// Allow a message coming back from the processor to say that we
		// are not done yet
		if (inputs.isBusy()) {
			logger.debug("Inputs still busy");
			return RepeatStatus.CONTINUABLE;
		}

		chunkContext.removeAttribute(INPUTS_KEY);
		chunkContext.setComplete();

		if (logger.isDebugEnabled()) {
			logger.debug("Inputs not busy, ended: " + inputs.isEnd());
		}
		return RepeatStatus.continueIf(!inputs.isEnd());

	}

}
~~~
- chunkProvider.provide() 로 Reader에서 Chunk Size 만큼 데이터를 가져옴
- chunkProcessor.process() 에서 Reader로 받은 데이터를 가공(Processor)하고 저장(Writer)함


- SimpleChunkProvider에 chunckProvicer.provide()
~~~java
@Override
	public Chunk<I> provide(final StepContribution contribution) throws Exception {

		final Chunk<I> inputs = new Chunk<I>();
		repeatOperations.iterate(new RepeatCallback() { //반복자

			@Override
			public RepeatStatus doInIteration(final RepeatContext context) throws Exception {
				I item = null;
				try {
					item = read(contribution, inputs);  // Reader.read()
					// 1건씩 데이터를 조회해 chunk size 만큼 데이터를 쌓는것이 provide() 가하는일
				}
				catch (SkipOverflowException e) {
					// read() tells us about an excess of skips by throwing an
					// exception
					return RepeatStatus.FINISHED;
				}
				if (item == null) {
					inputs.setEnd();
					return RepeatStatus.FINISHED;
				}
				inputs.add(item); //반복자가 끝날때까지 inputs에 추가
				contribution.incrementReadCount();
				return RepeatStatus.CONTINUABLE;
			}

		});

		return inputs;

	}

~~~

## SimpleChunkProcessor
process()

~~~java
@Override
	public final void process(StepContribution contribution, Chunk<I> inputs) throws Exception {

		// Allow temporary state to be stored in the user data field
		initializeUserData(inputs);

		// If there is no input we don't have to do anything more
		if (isComplete(inputs)) {
			return;
		}

		// Make the transformation, calling remove() on the inputs iterator if
		// any items are filtered. Might throw exception and cause rollback.
		Chunk<O> outputs = transform(contribution, inputs);

		// Adjust the filter count based on available data
		contribution.incrementFilterCount(getFilterCount(inputs, outputs));

		// Adjust the outputs if necessary for housekeeping purposes, and then
		// write them out...
		write(contribution, inputs, getAdjustedOutputs(inputs, outputs));

	}
~~~
- Chunk<I> inputs를 파라미터로 받음
    - 이 데이터는 chunkProvider.provide() 에서 받은 ChunkSize 만큼 쌓인 item
- transform() 에서는 전달받은 inputs을 doProcess() 로 전달하고 변환값을 받음
- transform()을 통해 가공된 대량의 데이터는 write() 를 통해 일괄 저장됨
    - write()는 저장이 될 수도있고, 외부 API로 전송할 수 도 있음
    - 이는 개발자가 itemWriter를 어떻게 구현했는지에 따라 달라짐  
    
transform()
~~~ java
protected Chunk<O> transform(StepContribution contribution, Chunk<I> inputs) throws Exception {
		Chunk<O> outputs = new Chunk<O>();
		for (Chunk<I>.ChunkIterator iterator = inputs.iterator(); iterator.hasNext();) {
			final I item = iterator.next();
			O output;
			try {
				output = doProcess(item); // 반복문을 통해 doProcess() 호출
			}
			catch (Exception e) {
				/*
				 * For a simple chunk processor (no fault tolerance) we are done
				 * here, so prevent any more processing of these inputs.
				 */
				inputs.clear();
				throw e;
			}
			if (output != null) {
				outputs.add(output);
			}
			else {
				iterator.remove();
			}
		}
		return outputs;
	}
~~~    

doProcess()
~~~ java
	protected final O doProcess(I item) throws Exception {

        // itemProcessor 없다면 item을 그대로 반환
		if (itemProcessor == null) { 
			@SuppressWarnings("unchecked")
			O result = (O) item;
			return result;
		}

        // 있다면 ItempProcessor의 process() 가공하여 반환
		try {
			listener.beforeProcess(item);
			O result = itemProcessor.process(item);
			listener.afterProcess(item, result);
			return result;
		}
		catch (Exception e) {
			listener.onProcessError(item, e);
			throw e;
		}

	}
~~~

가공된 데이터들은 doWrite() 호출해서 일괄처리
~~~java
	protected final void doWrite(List<O> items) throws Exception {

		if (itemWriter == null) {
			return;
		}

		try {
			listener.beforeWrite(items);
			writeItems(items); //
			doAfterWrite(items);
		}
		catch (Exception e) {
			doOnWriteError(e, items);
			throw e;
		}

	}
~~~ 

## Page Size vs Chunk Size
- Chunk Size 한번에 처리될 트랜잭션 단위
- Page Size는 한번에 조회할 item의 양

AbstractItemCountingItemStreamItemReader read()메소드
~~~ java
@Override
	public T read() throws Exception, UnexpectedInputException, ParseException {
		if (currentItemCount >= maxItemCount) {
			return null;
		}
		currentItemCount++;
		T item = doRead();
		if(item instanceof ItemCountAware) {
			((ItemCountAware) item).setItemCount(currentItemCount);
		}
		return item;
	}
~~~

AbstractPagingItemReader doRead()
~~~java
@Override
	protected T doRead() throws Exception {

		synchronized (lock) {

			if (results == null || current >= pageSize) {

				if (logger.isDebugEnabled()) {
					logger.debug("Reading page " + getPage());
				}

                // 현재 읽어온 데이터가 없거나 PageSize를 초과한 경우 호출
				doReadPage(); 
				
				page++;
				if (current >= pageSize) {
					current = 0;
				}

			}

			int next = current++;
			if (next < results.size()) {
				return results.get(next);
			}
			else {
				return null;
			}

		}

	}
~~~
- Page 단위로 끊어서 조회
    
JpaPagingItemReader의 doReadPage()의 코드 
~~~ java
@Override
	@SuppressWarnings("unchecked")
	protected void doReadPage() {

		EntityTransaction tx = null;
		
		if (transacted) {
			tx = entityManager.getTransaction();
			tx.begin();
			
			entityManager.flush();
			entityManager.clear();
		}//end if

		Query query = createQuery().setFirstResult(getPage() * getPageSize()).setMaxResults(getPageSize());

		if (parameterValues != null) {
			for (Map.Entry<String, Object> me : parameterValues.entrySet()) {
				query.setParameter(me.getKey(), me.getValue());
			}
		}

		if (results == null) {
			results = new CopyOnWriteArrayList<>();
		}
		else {
			results.clear();
		}
		
		if (!transacted) {
			List<T> queryResult = query.getResultList();
			for (T entity : queryResult) {
				entityManager.detach(entity);
				results.add(entity);
			}//end if
		} else {
			results.addAll(query.getResultList());
			tx.commit();
		}//end if
	}
~~~
- Reader에서 지정한 Page Size 만큼 offset, limit 값을 지정하여 페이징 쿼리를 생성 createQuery()  
사용 (query.getResultList())함
- 쿼리 실행 결과 results 에 저장  
- Page size는 페이징 쿼리에서 Page의 Size를 지정하기 위한 값
- 만약 2개의 값이 다르면 pageSize 10이고 ChunkSize 50이면 itemReader에서 Page조회가 5번 일어나면 1번의 트랜잭션 발생 chunk가 처리
~~~
Setting a fairly large page size and using a commit interval that matches the page size should provide better performance. 
(상당히 큰 페이지 크기를 설정하고 페이지 크기와 일치하는 커미트 간격을 사용하면 성능이 향상됩니다.)
~~~
**2개 값의 의미하는 것은 다르지만 일치시키는것이 좋은방법!!!!!!! 꼭 일치**
    
## 참고사이트
  - [Spring Batch 가이드 - Chunk 지향 처리](https://jojoldu.tistory.com/331?category=635883)
  - [Spring Batch Commit-interval](https://sheerheart.tistory.com/entry/Spring-Batch-commitinterval%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A0%95%EB%A6%AC)
