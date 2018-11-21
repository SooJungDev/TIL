## Spring data jpa에서 update query 사용하는 경우
~~~ java
 public interface QuestionRepository extends SlippCommonRepository<Question, Long>{
 	@Query("SELECT q from Question q JOIN q.tags t where t.name = :name")
 	Page<Question> findsByTag(@Param("name") String name, Pageable pageable);
 
 
 	@Modifying
 	@Query("UPDATE Question q set q.showCount = q.showCount + 1 where q.questionId = :questionId")
 	void updateShowCount(@Param("questionId") Long questionId);
 }
~~~
- @Query 어노테이션에서 updae 쿼리를 사용하는 경우 @Modifying 이라는 어노테이션 추가적으로 사용
- 사용하지 않는 경우 Not supported for DML operations 에러 발생

## 참고사이트
  - [Spring data jpa에서 update query 사용하는 경우](https://www.slipp.net/questions/95)