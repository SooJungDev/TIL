## 상수란?
- 상수는 변하지 않는 값임 
- 예를 들어 변하지 않는 상수값에 따라서 그값에 해당하는 과일의 의미를 고정한다고 가정.  그런데 주석으로 상수의 의미를 전달하고있지만 주석이 없어졌을 경우에는 상수를 사용하는 코드를 알아보기 힘들다.
- 이런 때에는 이름을 선언해 주는 것이 좋음 변수를 지정하고 그변수를 final 로 처리하면 한번 설정된 변수는 바뀌지않음 또한 바뀌지 않는 값이라면 인스턴스 변수가아니라 클래스 변수 static 으로 지정하는 것이 좋음
- private final static int APPLE=1;
- 다른내용 : switch 문을 사용할때는 switch의 조건으로 제한된 데이터 타입만 사용가능 byte,short,char,int, enum,String,Character,Byte,Short,Integer

## enum이란?
- 열거형이라고 부름(enumerated type)
- 열거형은 서로 연관된 상수들의 집합
- enum은 class, interface와 동급의 형식을 가진 단위
-  사실상 class이며 편의를 위해서 enum만의 위한 문법적 형식을 가지고있음, 구분하기 위해 enum 키워드 사용
  
 ## enum 사용하는 이유 
 - 코드가 단순해진다.
 - 인스턴스 생성과 상속을 방지
 - 키워드 enum을 사용하기 때문에 구현의 의도가 열거임을 분명하게 나타낼수 있음
  
 ## enum 문법 
  ~~~ java
  package org.opentutorials.javatutorials.constant2;
 /*
  class Fruit{
      public static final Fruit APPLE  = new Fruit();
      public static final Fruit PEACH  = new Fruit();
      public static final Fruit BANANA = new Fruit();
  }
  class Company{
      public static final Company GOOGLE = new Company();
      public static final Company APPLE = new Company();
      public static final Company ORACLE = new COMPANY(Company);
  }
  */

  // 위와 동일한 의미
  enum Fruit{
    APPLE,PEACH,BANANA
  }
  enum Company{
    GOOGLE,APPLE,ORACLE
  }
  
  public class ConstantDemo {
      
      public static void main(String[] args) {
         
         /* if(Fruit.APPLE == Company.APPLE){
              System.out.println("과일 애플과 회사 애플이 같다.");
          }
          */
           Fruit type = Fruit.APPLE;
          switch(type){
            case APPLE:
                System.out.println(57+" kcal");
                break;
            case PEACH:
                System.out.println(34+" kcal");
                break;
            case BANANA:
                System.out.println(93+" kcal");
                break;
          }
      }
  }
  ~~~

## enum과 생성자 
- enum 은 사실상 클래스이기 떄문에 생성자를 가질수 있음

~~~ java
  enum Fruit{
    APPLE("red"),PEACH("pink"),BANANA("yellow");
    private String color;
    public String getColor(){
      return this.color;
    }
    Fruit(String color){
      System.out,println("Call Constructor"+this);
      this.color = color;
    }
  }
  enum Company{
    GOOGLE,APPLE,ORACLE
  }

  public class ConstantDemo {
      
      public static void main(String[] args) {
           Fruit type = Fruit.APPLE;
          switch(type){
            case APPLE:
                System.out.println(57+" kcal"+Fruit.APPLE.getColor());
                break;
            case PEACH:
                System.out.println(34+" kcal"+Fruit.PEACH.getColor();
                break;
            case BANANA:
                System.out.println(93+" kcal"+Fruit.BANANA.getColor());
                break;
          }

          for(Fruit f : Fruit.values()){
            System.out.println(f);
          }
      }
  }
~~~
## 열거형의 특성
- 열거형은 연관된 값들을 저장함
- 또한 그값들이 변경되지 않도록 보장함
- 열거형 자체가 클래스이기 떄문에 열거형 내부에 생성자,필드 메소드를 가질수가 있어서 단순히 상수가 아니라 더많은 역할을 할 수 있음


## enum을 사용해서 switch/case 문 대신 구현하기 제

- 아래와 같이 구현하면 불필요한 switch/case 문을 생략할 수 있음 
- [java enum에서 값에 해당하는 enum 가져오기](https://www.slipp.net/questions/563) 링크에서 해당 코드 발췌
~~~java
enum Expression {
    PLUS("+"), MINUS("-"), TIMES("*"), DIVIDE("/");
    
    private String expression

    Expression(String expression){
      this.expression = expression;
    }

    static Expression of(String expression){
        return Arrays.stream(values())
                     .filter(v -> expression.equals(v.expression))
                     .findFirst()
                     .orElseThrow(() -> new IllegealArgumentException(String.format("%s 사칙연산에 해당되지 않은 표현식입니다.",expression)));
    }
}
~~~

## enum을 사용해서 if/else 문 대신 구현하기 예제

- 아래와 같이 구현하면 불필요한 if/else 문을 생략할 수 있음 
- [Java Enum 활용기](https://woowabros.github.io/tools/2017/07/10/java-enum-uses.html) 링크에서 해당 코드 발췌
~~~
public enum PayGroup {
    CASH("현금", Arrays.asList("ACCOUNT_TRANSFER", "TOSS")),
    CARD("카드", Arrays.asList("CARD", "KAKAO_PAY", "PAYCO")),
    ETC("기타", Arrays.asList("POINT", "COUPON")),
    EMPTY("없음", Collections.EMPTY_LIST);
    private String title;
    private List<String> payList;

    PayGroup(String title, List<String> payList) {
        this.title = title;
        this.payList = payList;
    }

    public static PayGroup findByPayCode(String code) {
        return Arrays.stream(values())
                .filter(payGroup -> payGroup.hasPayCode(code))
                .findAny()
                .orElse(EMPTY);
    }

    private boolean hasPayCode(String code) {
        return payList.stream()
                .anyMatch(pay -> pay.equals(code));
    }

    public String getTitle() {
        return title;
    }
}
~~~


## 참고사이트
- [생활코딩 enum 강의](https://opentutorials.org/course/1223/6091)


  