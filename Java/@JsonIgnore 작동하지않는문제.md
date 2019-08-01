## @JsonIgnore 작동하지않는문제
- 어노테이션을 달아줘도 작동이안됬었는데 ..........
- 기존 임포트 해준것 
~~~
import org.codehaus.jackson.annotate.JsonIgnore;
~~~



- 밑에처럼 임포트해주니까 잘되었음!!!!!!!
~~~
import com.fasterxml.jackson.annotation.JsonIgnore;
~~~
## 참고사이트
- [Spring @JsonIgnore not working](https://stackoverflow.com/questions/19894955/spring-jsonignore-not-working)