## @Cacheable
- 캐시 추상화 
    - 캐싱선언 : 캐시되어야 하는 메서드와 정책을 정한다
    - 캐시 구성 : 데이터를 저장하고 읽고 기반 캐시 
    
 
- @Cacheable 는 캐시 할 수 있는 메서드를 지정하는데 사용한다. 즉 어노테이션을 사용한 메서드는 결과를 캐시에 저장하므로 뒤이은 호출에는 실제로 메서드
를 실행하지 않고 캐시에 저장된 값을 반환한다.

~~~java
@Cacheable("books")
public Book findBook(ISBN isbn) {....}
~~~
- 가장 간단한 형식으로는 어노테이션이 붙은 메서드와 연관된 캐시의 이름만 있으면 어노테이션을 선언 할 수 있음


~~~java
@Cacheable({"books","isbns"})
public Book findBook(ISBN isbn) {....}
~~~
- 하나의 캐시만 선언하는 대부분의 경우 어노테이션은 여러 이름을 지정해서 한 캐시를 여러번 이용 할 수있음


### 키 생성
- 캐시는 본질적으로 키-밸류 저장소 이므로 캐시된 메서드를 호출 할때마다 해당 키로 변환되어야함
- 캐시 추상화는 다음 알고리즘에 기반을 둔 keyGenerator 를 사용한다.
      - 파라미터가 없으면 0을 반환한다.
      - 파라미터가 하나만 있으면 해당 인스턴스를 반환한다
      - 파라미터가 둘 이상이면 모든 파라미터의 해시를 계산한 키를 반환한다.
      
## 커스텀 키 생성 선언
- 캐싱은 일반적이므로 대상 메서드는 인자값이 두개가 있다면 모두 같아야 같은 캐쉬를 보내는데 리턴값에 영향을 미치는 요소가 page 인자값만 영향을 미치면
캐쉬 키 값을 별도로 설정할수있음

~~~ java
@Cacheable (key = "#page")
public List<String> getList(int page, String, query){

  .....
}
~~~    
- 위와 같이 설정하면 같은 page 값일 경우 query 에 어떤 인자가 들어오더라도 같은 캐쉬값을 반환함

- 아래와 같은 코드 캐시가 정상적으로 동작하지 않음!!!
~~~java
class Person
{
    private String name;
    
    public String getName(){return name;}
}

@Cacheable(key = "#kim")
public List<String> getList(Person kim){
....
}
~~~ 
- **작동하지 않음!!!!!!!**
   - 객체의 native 값을 이용하거나 **Object일 경우 hashCode() 만을 사용하여 키 값을 생성하는데 Object 의 hashCode는 객체에서 재정의하지 않은 이상
     무조건 다른값**이 들어감
- spring 4.0 이후 버젼에서는 SimplekeyGenerator 를 사용하며 hashCode 만이 아닌 복합키를 사용함

- 인자 값이 Object 객체인 경우 객체내 값을 이용하여 키값을 쓸 수 있음 (내부도 native 값이거나 String 같은 heap 내에 주소가 유일한 경우에만 가능)
~~~java
@Cacheable(key = "#kim.name")
public List<String> getList(Person kim){
~~~      
      

## 참고사이트
- [Spring 레퍼런스 28장 캐시 추상화](https://blog.outsider.ne.kr/1094)
- [@Cacheable key값 정하기](https://jistol.github.io/java/2017/02/09/springboot-cache-key/)