## 빌던 패턴(Builder Pattern)
- 텔리스코핑 생성자 패턴운 선택 가능한 매개변수가 많아질 경우 신축성 있게 처리하지 못함
- 텔리스코핑 생성자 패턴은 매개변수가 많을 때클라이언트 코드 작성이 힘들고 코드의 가독성도 떨어짐
- 자바빈즈 패턴 심각한 단점
    - 여러번의 메소드 호출로 나누어져 인스턴스가 생성, 생성과정을 거치는 동안 자바빈 객체가 일관된 상태를 유지하지못할수도 있음
    - 불변(immutable) 클래스를 만들 수 있는 가능성 배제 하므로 스레드에서 안정성을 유지하려면 프로그래머의 추가적인 노력
- 이런 단점 위와같은 텔리스코핑 생성자 패턴의 안전성과 자바빈즈 패턴의 가독성을 결합한 세번째 방법이 있음
    - 그게바로 Builder 패턴
    
빌더패턴은??
- 원하는 객체를 바로 생성하는 대신 클라이언트는 모든 필수 매개변수를 갖는 생성자 (또는 static 팩토리 메소드)를 호출하여  빌더 객체를 얻는다.
- 그다음에 빌더 객체의 세터 메소드를 호출 하여 필요한 선택 메개변수들의 갑슬 설정한다.
- 마지막으로 클라이언트는 매개변수가 없는 build 메소드를 호출하여 불변 객체를 생성한다
   
~~~java
//빌더 패턴
public class NutritionFacts{
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;
    
    public static class Builder {
        // 필수 매개변수들
        private final int servingSize;
        private final int serving;
        
        //선택 매개변수들 - 디폴트값으로 초기화 한다.
        private int calories=0;
        private int fat=0;
        private int carbohydrate=0;
        private int sodium=0;
        
        public Builder(int servingSize, int servings){
            this.servingSize=servingSize;
            this.servings=servings
        }
        
        public Builder calories(int val){
            calories = val;
            return this;
        }
        
        public Builder fat(int val){
            fat = val;
            return this;
        }
        
        public Builder carbohydrate(int val){
            carbohydrate = val;
            return this;
        }
        public Builder sodium(int val){
             sodium = val;
             return this;
        }
        
        public NutritionFacts build(){
            return new NutritionFacts(this);
        }
    
    }
    
    private NutritionFacts(Builder builder){
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }

}
~~~
- NutritionFacts 는 불변 클래스이며, 모든 매개변수의 디폴트 값을 한군데 모아 둔다는 것에 주목
- 빌더의 세터 메소드는 연속적으로 여러번 호출 될 수 있도록 빌더 자신의 객체를 반환한다.

- 다음 클라이언트 코드
~~~java

NutritionFacts cocaCola = new NutritionFacts.Builder(240.8)
    .calories(100).sodium(35).carbohydrate(27).build();
~~~
- 작성이 쉽고 가독성이 좋음
- 생성자처럼 빌더는 자신의 매개변수 불변규칙을 적용 할수 있음
- build 메소드는 그런 불변 규칙을 검사 할 수 있음 
    - 불변규칙에 위배될 경우 IllegalStateException 예외를 발생시키며 이 예외 관련 메소드에서 어떤 불변 규칙이 위배되어있는지 알려주어야한다.
- 매개변수를 받는 세터 메소드를을 두는 것 만일 불변 규칙이 충족되지 않으면 세터메소드에서 IllegalArgumentException 예외를 발생시킴
    - 이렇게 하면 build 메소드가 호출 될 때가지 기다릴 필요 없고, 부적합한 매개변수가 전달되는 즉시 불변 규칙의 이상 유무를 검출 할 수있다는 장점
 - 여러개의 가변인자 매개변수를 가질 수 있다는 것임
 - 빌더는 각 매개변수에 대해 별도의 세터 메소드를 사용하므로, 우리가 필요한 만큼 많은 가변인자를 가질 수있음 
 - 빌더 패턴은 유연성이 좋음
    - 하나의 빌더는 여러개의 객체를 생성하는데 사용 할 수 있음 매개변수는 다양하게 조정될 수 있음
    - 빌더를 사용하면 일부 필드의 값을 자동으로 설정 할 수 있음 
    
- 매개변수들의 값이 설정한 빌더는 훌륭한 추상팩토리를 만든다,.
    - 즉 클라이언트 코드에서 그런 빌더를 메소드로 전달하여, 메소드 하나의 이상의 객체를 생성 할 수 있다.
    - 빌더가 생성하는 객체의 타입이 필요하다.
 
 ~~~java
 public interface Builder<T>{
    public T build();
 }
 ~~~
 - NutritionFacts.Builder 클래스에서 Builder<NutritionFacts> 인터페이스를 구현하게끔 할 수 있다는 것에 주목
 
 ㅡ 특정 빌더 인스턴스를 매개변수로 받는 메소드에서는 빌더의 타입 매개변수로 바운드 와일드카드 타입을 사용한다
 ~~~java
 Tree buildTree(Builder<? extends Node> nodeBuilder){...}
 ~~~
 
 - 자바에서는 Class 란 이름의 클래스가 추상 팩토리 패턴을 구현하고 있는데 이 클래스의 newInstanse 메소드에서 빌드 메소드의 역할을 수행한다.
 - newInstance 메소드는 항상 객체가 생성 될 클래스의 매개변수 없는 생성자를 호출하려 한다.
    - 매개변수가 없는 생성자를 디폴트 생성자라고 하며 우리가 명시적으로 클래스에 정의하지 않으면 컴파일 시점에 자바 컴파일러가 자동으로 추가
        - 하지만 우리가 생성자를 하나라도 정의하면 디폴트 생성자를 자동으로 만들지 않으므로 이때는 디폴트 생성자도 반드시 같이 정의해주어야함
        - 서브클래스의 객체를 생성할댸 슈퍼클래스의 디폴트 생성자가 자동으로 호출되기 때문
    - 컴파일에러가 생기지 않음 
    - 객체를 생성하는 클라언트 코드에서 런타임시 InstantiationException 이나 IllegalAccessException 예외 발생에 대처해야 한다       
    - 즉 Class 클래스의 newInstance 메소드는 컴파일 시점의 예외 검사를 어렵게 만들며 실행할떄 예외를 발생시킬 수 있음
    - 빌더패턴은 이런 결함을 해소시켜줌
- 요약하면, 생성자나 static 팩토리 메소드에서 많은 매개변수를 갖게 될 클래스를 설계할 떄는 빌더 패턴의 좋은 선택 특히 선택 매개변수가 대부분인 경우가 그럼
- 가독성이 좋고 안전하다!!!
    

Lombok @Builder
- 롬복의 @Builder 애노테이션으로 쉽게 사용가능
~~~java
@Builder
public class NutritionFacts{
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final sodium;
    private final int carbohydrate;
}
~~~

다음과 같이 사용가능
~~~java
NutritionFact.NutritionFactsBuilder builder = NutritionFacts.builder();
builder.calories(230);
builder.fat(10);
NutritionFacts facts = builder.build();
~~~

체인도가능
~~~java
NutiritionFacts facts = NutritionFacts.builder()
    .calories(230)
    .fat(10)
    .build();
~~~




## 참고자료
- 이펙티브자바 책 참고 
- [빌더 패턴(Builder Pattern)](https://johngrib.github.io/wiki/builder-pattern/#effective-java%EC%9D%98-%EB%B9%8C%EB%8D%94-%ED%8C%A8%ED%84%B4)
    - 위에 사이트에 자세히 설명나와있고 잘 정리해주심~