## JPA Entity 예제코드
~~~ java
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Getter
@Entity
public class Posts {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;


    @Column(length = 500, nullable = false)
    private String title;

    @Column(columnDefinition = "TEXT", nullable = false)
    private String content;


    private String author;

    @Builder
    public Posts(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
~~~

## JPA 어노테이션
- @Entity
  - 테이블과 링크될 클래스임을 나타냄
  - 언더스코어 네이밍으로 이름을 매칭 ex)SalesManager.java -> sales_manager table
- @Id
    - 해당 테이블의 pk 필드를 나타냄  
- @GeneratedValue
  - pk의 생성 규칙을 나타냄
  - 기본 값은 AUTO로 mysql의 auto_increment와 같이 자동 증가하는 정수형값
  - 스프링부트2에서는 옵션을 추가하셔야만 auto_increment가 됨
  - 2에서는  @GeneratedValue(strategy = GenerationType.IDENTITY)
  - 아니면 application.yml에 use-new-id-generator-mappings을 false로 설정
- @Column
  - 테이블의 컬럼을 나타내며느 귣이 선언하지 않더라도 해당클래스의 필드는 모두 컬럼이 됨
  - 사용하는 이유는 기본값 외에 추가로 변경이 필요한 옵션이 있을 경우 사용  
  - 문자열의 경우 VARCHAR(255)가 기본값인데, 사이즈를 500으로 늘리고 싶거나, 타입을 TEXT로 변경하고 싶거나 
   (ex : content)등에 사용됨

## Lombok 어노테이션
- @NoArgConstructor: 기본 생성자 자동추가
    - access = AccessLevel.PROTECTED: 기본 생성자 접근 권한을 protected로 전환
    - 생성자로 protected Posts(){}와 같은 효과
    - Entity 클래스를 프로젝트 코드상에서 기본생성자로 생성하는것은 막고, JPA 에서 Entity 클래스를 생성하는 것은 허용하기 이해 추가
- @Getter : 클래스내 모든 필드의 Getter 메소드를 자동생성
- @Builder : 해당 클래스의 빌더패턴 클래스를 생성
    - 생성자 상단에 선언시 생성자에 포함된 필드만 빌더에 포함 

## JPA에서 DB layer 접근자
~~~ java
public interface PostsRepository extends JpaRepository<Posts,Long> {
}
~~~    
- 보통 mybatis 등에서 Dao라고 부르는 DB Layer 접근자
- JPA에서 Repository라고 부르며 인터페이스로 생성
- 인터페이스 생성후  JpaRepository<Entity 클래스,PK 타입>을 상속
- 기본적인 CRUD 메소드가 자동생성
- @Repository를 추가할 필요X 

## BDD(Behaviour-Driven Development)
- given
  - 테스트 기반 환경을 구축하는 단계
- when
  - 테스트 하고자 하는 행위 선언
- then
 - 테스트 결과 검증
 
 ## Bean 주입방식
 spring framework에서 bean 주입시 세가지 방법
 - @AutoWired (비권장)
 - setter
 - 생성자(가장 권장하는 방법)
   - *생성자로 쉽게 주입하려면* lombok @AllArgsConstructor 사용
       모든 필드를 인자 값으로 하는 생성자를 대신 생성   
 - lombok을 사용하는 이유: 해당클래스의 의존성 관계가 변경 될 때마다 생성자코드를 계속 수정하는 번거로움 해결
 
 ## Entity 클래스와 Controller에서 쓸 DTO 분리
 -  **절대로 테이블과 매핑되는 Entity클래스를 Request/Response 클래스로 사용되서는 안됨**
 - Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 Request와 Response용 DTO는 view를 위한 클래스라 자주 변경 
 - view layer, db layer 철저하게 분리하는 것이 좋음
 
 ## h2 콘솔 사용
 application.yml에  해당 설정을 추가해줌
 ~~~ yml
 spring:
   h2:
     console:
       enabled: true
 ~~~
- 브라우저에서 http://localhost:8080/h2-console 입력해 접속
- JDBC URL에  jdbc:h2:mem:test 와 같이 jdbc:h2:mem:testdb가 아닌 다른 이름으로 넣으면 로그인은 가능하나 테이블이 보이지 않음!!!

## JPA Auditing
- 보통 Entity에 해당 데이터의 생성시간 수정시간 포함시킴
- 매번 DB에 insert 하기전 update 하기전에 날짜 데이터를 등록/수정하는 코드가 여기저기 들어감
- 단순하고 반복적인 코드가 모든 테이블과 서비스 메소드에 포함되어야 된다고 했을때 이문제를 해결하기 위해 JPA Auditing 사용
~~~ java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity {

    @CreatedDate
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
~~~
 - @MapperdSuperclass
   - JPA Entity 클래스들이 BaseTimeEntity을 상속할 경우 (createdDate, modifiedDate)도 컬럼으로 인식하도록 함
 - @EntityListeners(AuditingEntityListener.class)
   - BaseTimeEntity 클래스에 Auditing 기능을 포함시킴
 - @CreatedDate
   - Entity가 생성되어 저장 될때 시간이 자동 저장
 - @LastModifiedDate
  - 조회한 Entity값을 변경 할때 시간이 자동저장됨
  
## service 메소드는 entity를 바로 받지 않는다.
- controller와 service 역할을 분리해줘야함
- 비지니스 로직 & 트랜잭션 관리는 모두 service에서 함
- view와 관련된 부분 controller에서 담당하도록한다
- 트랜잭션??
  - DB 데이터를 등록.수정.삭제 하는 service 메소드는 @Transactional을 필수적으로 가져감
  - 메소드 내에서 exception이 발생하면 해당 메소드에서 이루어진 모든 db작업을 초기화시킴
  
 
## 참고사이트
  - 밑에 사이트를 보고 천천히 따라해보고 정리함.. 정말잘되있음
  - [JPA로 간단한 API 만들기](https://jojoldu.tistory.com/251?category=635883)
  - [h2 테이블 안보일때](https://www.popit.kr/spring-boot%EC%97%90%EC%84%9C-jpa%EC%99%80-spring-data-%ED%99%9C%EC%9A%A9/)