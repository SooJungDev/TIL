## Decorator pattern
- 데코레이터 패턴에서는 객체의 추가적인 요건을 동적으로 추가한다. 
- 데코레이터는 서브클래스를 만드는 것을 통해서 기능을 유연하게 확장 할 수 있는 방법을 제공
- 쉽게 말하면 원본에 대해서 무언가를 더 입혀서 새로운것을 만든다
- Decorator 는 Proxy 패턴과 똑같음 Proxy 패턴은 반환 값을 수정 하지 않고 온전하게 반환 Decorator 는 반환 값을 조작해서 반환함 

단점
- 데코레이터 패턴을 이용해 디자인을 하다보면 잡다한 클래스들이 많아질 수 있음
    - 객체의 정체를 알기 힘듬
 
 핵심정리
 - 상속을 통해 확장을 할 수 도 있지만 디자인 유연성 면에서는 좋지 않다.
 - 기존 코드를 수정 하지 않고도 행동을 확장하는 방법이 필요하다
 - 구성과 위임을 통해서 실행중에 새로운 행동을 추가 할 수 있다.
 - 상속 대신 데코레이터 패턴을 이용해서 행동을 확장 할 수 있다
 - 데코레이터 패턴에서는 구상 구성요소를 감싸주는 데코레이터들을 사용한다.
 - 데코레이터의 수퍼 클래스는 자신이 장식하고 있는 객체의 수퍼클래스와 같다
 - 데코레이터 패턴을 사용하면 자잘한 객체들이 많이 추가 될 수 있고, 데코레이터를 너무 많이 사용하면 코드가 필요 이상으로 복잡해 질 수 있음
 
 OCP Open Closed Principle
 - OCP는 가장 중요한 디자인 원칙 가운데 하나다.
 - 클래스는 확장에 대해서는 열려 어야 하지만 코드 변경에 있어서 닫혀 있어야 한다.
 - 즉 기존코드는 건드리지 않은 채로 확장을 통해서 새로운 행동을 간단하게 추가 할 수 있도록 하면, 새로운 기능을 유연하게 추가 할 수 있어 주변 환경에 잘 적응 할 수 있으면서도
 강하고 튼튼한 디자인을 만들 수 있다.
 
 
 ## 이해 할 수 있는 간단한 예제

- Cake.java
~~~java
public class Cake{
   public String getCake() {
        return "케이크";
   }

}
~~~

- CreamDecorator.java
~~~java
public class CreamDecorator extends Cake{
    private Cake cake;

    public CreamDecorator(Cake cake){
        this.cake = cake;
    }

   @Override
   public String getCake(){
        return  " *생크림* "+cake.getCake()+" *생크림* "
   }
}
~~~

- StrawberryDecorator.java
~~~java
public class StrawberryDecorator extends Cake{
    private Cake cake;
    
   public StrawberryDecorator(Cake cake){
    this.cake = cake;  
  } 
 @Override
 public String getCake(){
      return  " * @딸기@ "+cake.getCake()+" @딸기@ "
 }
}
~~~

- Main.java
~~~java
public Class Main{
    public staic void main(String[] args){
        Cake cake = new Cake();
        System.out.println(cake.getCake()); // 케이크
          

       CreamDecorator creamCake = new CreamDecorator(cake);
       System.out.println(creamCake.getCake()); strawberryCake // *생크림* 케이크 *생크림*

       StrawberryDecorator strawberryCake = new StrawberryDecorator(creamCake);
       System.out.println(strawberryCake.getCake()); // @딸기@ *생크림* 케이크 *생크림* @딸기@
    }
}
~~~

- 메소드를 호출하면서 반환 값에 추가적으로 더 덧붙여서 결과값을 반환한다.

 참고사이트
 - [데코레이터 패턴(Decorator Pattern)과 프록시 패턴](https://lalwr.blogspot.com/2016/02/decorator-pattern.html)
 - [데코레이터 패턴(Decorator Pattern)](https://limkydev.tistory.com/80)