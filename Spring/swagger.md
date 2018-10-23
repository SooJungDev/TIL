## Springboot swagger 추가하기
- maven 사용시 
  ~~~ 
    <!-- Swagger --> 			
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.6.1</version>
    </dependency>
            
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.6.1</version>
    </dependency>
  ~~~

- swagger configration 설정하는 자바파일을 만들어준다
  
  ~~~ java
       @Configuration
       @EnableSwagger2
       public class SwaggerConfig {
        @Bean
        public Docket api() {
            return new Docket(DocumentationType.SWAGGER_2)
                    .select()
                    .apis(RequestHandlerSelectors.any()) // 현재 RequestMapping으로 할당된 모든 URL 리스트를 추출
                    .paths(PathSelectors.ant("/api/**")) // 그중 /api/** 인 URL들만 필터링
                    .build();
                    }
        }
  ~~~


## 참고 사이트

- http://yookeun.github.io/java/2017/02/26/java-swagger/

- https://jojoldu.tistory.com/31