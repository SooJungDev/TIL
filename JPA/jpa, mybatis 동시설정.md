## 설정방법
- jpa 도 복잡한 쿼리 생성 가능하지만 진짜 가독성너무떨어짐... 난못보겟음 ㅠㅠ
    - 이럴때 mybatis 추가해줌 더보기 쉬운듯
- jpa 설정되어있는 프로젝트(spring boot)로 되어있는 가정
- Mybatis 설정을 추가해준다 별다를꺼없음!!!! 

- pom.xml
~~~
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.0.1</version>
</dependency>
~~~

- gradle 일경우 
~~~

compile("org.mybatis.spring.boot:mybatis-spring-boot-starter:2.0.1")
~~~

Mapper
- JPA 의 Repository interface에 대응되는 Mapper interface 정의
- @Mapper ,@Repository 붙여준다
   - @Mapper 어노테이션을 정의하면 Mybatis의 SqlSessionFactory 가 해당 어노테이션을 찾아봐 Mapper의 구현체를 정의하고 관리해줌
- @Select , @Insert, 등등 붙여서 사용 (xml 파일 설정 따로해줄수 있으나 이런식으로 관리하는게 더나은거같음)
~~~
@Repository
@Mapper
public interface MybatisExampleMapper{

  @Select("select * from example")  
  List<Map <String, Object>> getExample();

}
~~~

- 해당 mapper를 주입받아서 사용 Service 단에서 주입받아서 사용
    - @Autowird 방법 3가지중 생성자로 생성하는것이좋음 롬복으로 편하게! @AllArgsConstructor
    - Mapper 사용은 별다른 구현없이 Mybatis의 SqlSessionFactory가 @Mapper 가 정의되어잇는 객체를 찾아와 정의한 구현체를 그대로 사용하면됨

~~~
@Service
@AllArgsConstructor
public class ExampleService{
    
    MybatisExampleMapper mybatisExampleMapper
    
    public List<Map<String, Object>> getEmployee() {
         return mybatisExampleMapper.getExample();
     }

}
~~~

## 참고사이트
- [JPA 환경에서 MyBatis 를 사용하여 데이타를 가지고 오기](https://jogeum.net/10)
- [SpringBoot + MyBatis + Gradle + MySql](https://medium.com/cashwalk/springboot-mybatis-gradle-mysql-7090359d5427)