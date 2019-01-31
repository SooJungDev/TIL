## Event 생성 API 구현: 테스트만들기 
스트링 부트 슬라이스 테스트
- @WebMvcTest
 - MockMvc 빈을 자동 설정 해준다. 따라서 그냥 가져와서 쓰면됨
 - 웹 관련 빈만 등록해준다.(슬라이스)
 
 MockMvc
 - 스프링 MVC 테스트 핵심클래스
 - 웹서버를 띄우지 않고도 스프링 MVC(DispatcherServlet)가 요청을 처리하는 과정을 확인 할 수 있기때문에 컨트롤러 테스트용으로 자주 쓰임
 
 테스트 할것
 - 입력값들을 전달하면 JSON 응답으로 201이나오는지 확인
     - Location 헤더에 생성된 이벤틀르 조회 할 수 있는 URI 담겨 있는지 확인
     - Id는 DB에 들어갈때 자동생성된 값으로 나오는지 확인
- 입력 값으로 누가 id나 eventStatus, offline, free 이런 데이터까지 같이주면
    - Bad_Request로 응답 VS 받기로 받기로 한 값이외는 무시
- 입력 데이터가 이상한 경우 Bad_Request로 응답
    - 입력값이 이상한 경우 에러
    - 비지니스 로직으로 검사 할 수 있는 에러
    - 에러 응답 메세지에 에러에 대한 정보가 있어야함
- 비지니스 로직 적용 했는지 응답 메세지 확인
    -  offline 과 free 값 확인
- 응답에서 HATEOAS 와 profile 관련 링크가 있는지 확인
    - self(view)
    - update(만든 사람은 수정 할 수 있으니까)
    - events(목록으로 가는 링크)
- API 문서 만들기 
    - 요청 문서화
    - 응답 문서화
    - 링크 문서화
    - profile 링크 추가    
    
  ## Event 생성 API 구현: 201 응답받기
  @RestController
  - @ResponseBody 를 모든 메소드에 적용한것과 동일하다.
  
  ResponseEntity를 사용하는 이유
  - 응답 코드, 헤더, 본문, 모두 다루기 편한 API
  
  Location URI 만들기  
  - HATEOS가 제공하는 linkTo(), methodOn() 사용
  
  객체를 JSON으로 변환
  - ObjectMapper 사용
  
  테스트 할것 
  - 입력값들을 전달하면 JSON 응답으로 201이 나오는지 확인
    - Location 헤더에 생성된 이벤트를 조회 할 수 있는 URI가 담겨 있는지 확인
    - id는 DB에 들어갈때 자동생성된 값으로 나오는지 확인
  
 ## Event 생성 API구현 :EventRepository 구현
 스프링 데이터 JPA
 - JpaRepository 상속받아 만들기
 
 Enum을 JPA 맵핑시 주의할것
 - @Enumerated(EnumType.STRING) 
   - 위에처럼 사용해주는 것이 좋음
   - 위에 처럼 선언하면 Enum의 name이 저장됨
   - EnumType.ORDINAL : enum 순서 값을 DB에 저장
 
 @MockBean
 - Mockito 를 사용해서 mock 객체를 만들고 빈으로 등록해줌.
 - (주의) 기존 빈을 테스트용 빈이 대체한다
 
 테스트 할것 
 - 입력 값들을 전달하면 JSON 응답으로 201이 나오는지 확인
    - Location 헤더에 생성된 이벤트를 조회 할 수 있는 URI 담겨있는지 확인
    - id는 DB에 들어 갈 떄 자동생성된 값으로 나오는지 확인
  
 ## 입력값 제한 하기
 - id 또는 입려받은 데이터로 계산 해야 하는 값들은 입력을 받지 않아야한다.
 - EventDto 적용
 
 DTO -> 도메인 객체로 값 복사
 - ModelMapper
 ~~~    
<dependency>
    <groupId>org.modelmapper</groupId>
    <artifactId>modelmapper</artifactId>
    <version>2.3.1</version> 
</dependency>
 ~~~
 
 통합 테스트로 전환
 - @WebMvcTest 빼고 다음 애노테이션 추가
    - @SpringBootTest
    - @AutoConfigureMockMvc
 - Repository @MockBean 코드 제거 
 
 테스트 할것 
  - 입력 값으로 누가 id,eventStatus, offline, free 이런 데이터까지 같이주면
    - Bad_Request 로 응답 vs 받기로 한 이외는 무시
    
 ## 입력값 이외에 에러 발생
 ObjectMapper 커스터마이징
 - spring.jackson.deserialization.fail-on-unknown-properties=true
 
## Bad Request 처리하기
@Valid와 BindingResult(또는 Errors)
- BindingResult는 항상 @Valid 바로 다음 인자로 사용해야함.(스프링 MVC)
- @NotNull, @NotEmpty, @Min, @Max 를 사용해서 입력값 바인딩 할때 에러 확인 할수 있음

도메인 Validator 만들기
- Validator 인터페이스 없이 만들어도 상관없음
테스트 설명용 애노테이션 만들기
- @Target, @Retention

테스트 할것 
- 입력한 데이터가 이상한 경우 Bad_Request로 응답
    - 입력값이 이상한 경우 에러
    - 비지니스 로직으로 검사 할 수있는 에러
    - 에러 응답 메세지에 에러에 대한 정보가 있어야한다.
  
 ## Bad Request 응답 본문만들기
 커스텀 JSON Serializer 만들기
 - extends JsonSerializer<T>(Jackson JSON제공)
 - @JsonComponent(스프링 부트 제공)
 
 BindingError
 - FieldError와 GlobalError(ObjectError)가 있음
 - objectName
 - defaultMessage
 - code
 - field
 - rejectedValue
 
## 매개변수를 이용한 테스트
테스트 코드 리팩토링
- 테스트에서 중복 코드 제거
- 매개변수만 바꿀수있으면 좋겠는데?!?!
- [JunitParams](​https://github.com/Pragmatists/JUnitParams)

~~~
 <!-- https://mvnrepository.com/artifact/pl.pragmatists/JUnitParams--> 
 <dependency>
    <groupId>pl.pragmatists</groupId>
    <artifactId>JUnitParams</artifactId>
    <version>1.1.1</version>
    <scope>test</scope>
 </dependency>
~~~
 
   