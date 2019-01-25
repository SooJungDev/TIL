## Java 8 Optional
- 자바 8에서는 Optional<T> 클래스를 이용해서 NullPointerException을 방지 할 수 있음 
- Optional<T> 클래스는 한마디로 null 이 올 수 있는 값을 감싸는 래퍼 클래스로 참조하더라도 null이 일어나지 않도록 해주는 클래스

~~~ java
public final class Optional<T>{
  // if non-null, the value; if null, indicates no value is present
  private final T value; // value 에  값을 저장 null이라도 바로 참조시 NPE 안남, 클래스여서 각종메소드 제공
}
~~~
## 생성하기
~~~ java
Optional<String> optional = Optional.empty();
System.out.println(optional); // Optional.empty
System.out.println(optional.isPresent()); // false
~~~
- null 이 올 수 있는 값을 Optional<T> 로 감싸서 생성가능 
- orElse , orElseGet 메소드를 이용해서 값이 없는 경우라도 안전하게 값을 가져 올 수 있음

~~~java
// Optional 안에는 값이 있을 수 있고 빈 객체일수도 있다.
Optional<String> optional = Optional.ofNullable(getString());

String result = optional.orElse("other"); // 값이 없다면 "other" 리턴
~~~

## java8 이전 코드 Optional 로 변경된 코드 예제
~~~ java
// 자바 8 이전 
List<String> list = getList();
List<String> listOpt = list ! =null ? list : new ArrayList<>();
~~~

- Optional<T> 와 Lambda 를 이용하면 좀 더 간단하게 표한 할 수 있음 
~~~
List<String> listOpt = Optional.ofNullable(getList()).orElseGet(() -> new ArrayList<>());
~~~

~~~ java
// null 체크 때문에 지저분해진 코드
User user = getUser();
if( User ! =null){
    Address address = user.getAddress();
    if( address !=null){
     String street = address.getStreet();
     if( street ! =null){
            return street;
        }
    }
}
return "주소없음";
~~~

map 메소드 해당 값이 null 아니면 mapper를 이용해 계산한 값을 저장하는 Optional 객체를 리턴
만약에 값이 null 이라면 빈 Optional 객체를 리턴

~~~ java
public<U> Optional<U> map(function<? super T, ? extends U> mapper)
public<U> Optional<U> flatMap(function<? super T, Optional<U> mapper)
~~~

map 메소드를 이용해 간단히 표현
~~~ java
Optional<User> user = Optional.ofNullable(getUser());
Optional<Address> address = user.map(User::getAddress);
Optional<String> street = address.map(Address::getStreet);
String result = street.orElse("주소 없음");

// 다음과 같이 축약해서 쓸 수 있다.
user.map(User:: getAddress)
    .map(Address :;getStreet)
    .orElse("주소 없음");
~~~

만약 위에서 getAddress와 getStreet 가 Optional<T> 객체를 리턴한다면 flatMap 메소드 사용 할 수 잇음
~~~ java
Optional<User> user = Optional.ofNullable(getUser());
String result = user
                .flatMap(User::getAddress)
                .flatMap(Address::getStreet)
                .orElse("주소 없음");
~~~

- NullPointerException을 핸들링하는 예제
~~~ java 
String value = null;
String result ="";
try{
  result = value.toUpperCase();
}catch (NullPoiterException e){
    throw new CustomException();
}
~~~

Optional <T> 를 이용하면 다음과 같이 표현 할 수 있음
~~~
String value = null;
Optional<String> valueOpt = Optional.ofNullable(value);
String result = valueOpt.orElseThrow(CustomException::new).toUppserclase();
~~~

## 참고사이트
- 해당 블로그에 잘 정리 되어있어서.. 보고 학습할수있었음
  - [Java 8 옵셔널 Optional](https://futurecreator.github.io/2018/08/14/java-8-optional/)