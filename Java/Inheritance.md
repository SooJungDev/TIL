## 상속
- 상속이란? 부모가 가진것을 자식에게 물려주는것을 의미한다.
    - 노트북은 컴퓨터의 한 종류다.
    - 침대는 가구의 한 종류이다. 혹은 침대는 가구다.
    - 소방차는 자동차다.
이렇게 말할 수 있는 관계를 is a 관계 혹은 kind of 관계라고 한다.

## Car를 상속받은 Bus를 class로 표현하는 방법
~~~ java
public class Car{

}

public class Bus extends Car{

}
~~~     
- 자바 클래스 이름 뒤에 extends 키워드를 적고 부모클래스 이름을 적게되면 부모클래스가 가지고 있는것을 상속 받을 수 있게된다.
- 상속이란 부모가 가지고 있는것을 자식이 물려받는것을 말한다. 즉, 부모가 가지고 있는것을 자식이 사용 할 수 있게된다.

## 부모 클레스에 메소드 추가하기
- Car에 run() 메소드를 추가
~~~ java
public class Car{
    public void run(){
        System.out.println("달리다.");
    }

}
~~~

- Car를 상속받은 Bus 사용
~~~ java
public class BusExam{
    public static void main(String args[]){
        Bus bus = new Bus();
        bus.run();
        // Bus class는 아무런 코드를 가지지않는다. 그럼에도 run이라는 메소드 사용하는데 문제되지 않음
    }

}
~~~

- Bus에 메소드 추가
~~~ java
public class Bus extends Car(){
    public void ppangppang(){
        System.out.println("빵빵");
    }
}
~~~
- Bus는 Car에서 물려받은 run메소드와 ppangppang 메소드를 사용 할 수 있게된다.
- 부모가 가지고 있는 메소드외에 추가로 메소드를 선언하는 것을 확장하였다고 표현한다.

## super와 부모생성자
- class가 인스턴스화 될 때 생성자가 실행되면서 객체의 초기화를 한다. 
- 그때 자신의 생성자만 실행 되는 것이 아니고 부모의 생성자부터 실행된다.

부모생성자
~~~java
public class Car{
  public Car(){
    System.out.println("Car의 기본생성자입니다.");
  }
}


public class Bus extends Car{
  public Bus(){
     System.out,println("Bus의 기본생성자입니다.");
  }
}
~~~

- 생성자 테스트
~~~
public class BusExam{
    public static void main(String args[]){
        Bus b = new Bus();
    }
}
~~~

결과
~~~
Car의 기본생성자입니다.
Bus의 기본생성자입니다.
~~~
- new 연산자로 Bus객체를 생성하면, Bus 객체가 메모리에 올라갈때 부모인 Car도 함께 메모리에 올라간다.
- 생성자는 객체를 초기화 하는 일을 한다.
- 생성자가 호출 될때 자동으로 부모이 생성자가 호출되면서 부모객체를 초기화 하게된다.

## super
- 자신을 가리키는 키워드가 this 라면, 부모를 가리키는 키워드는 super
- super()는 부모의 생성자를 의미한다.
- 부모의 생성자를 임의로 호출하지 않으면, 부모 class의 기본 생성자가 자동으로 호출된다.
- 아래 예제처럼 호출해보면 위에서 super()를 호출하지 않을때와 결과가 같다.

~~~ java
public Bus(){
    super();
    System.out,println("Bus의 기본생성자입니다.");
}
~~~

## 부모의 기본생성자가 아닌 다른 생성자를 호출하는 방법
- 클래스는 기본 생성자가 없는 경우도 존재한다.
~~~java
public class Car{
    public Car(String name){
        System.out.println(name+" 을 받아들이는 생성자입니다.");
    }
}
~~~
- Car class가 위처럼 수정되면 , Bus생성자에서 컴파일 오류가 발생한다.
- 부모가 기본생성자가 없기 때문에 컴파일 오류가 발생하게 되는것이다.
- 이런 문제를 해결하려면 자식 클래스의 생성자에서 직접 부모의 생성자를 호출해야 합니다.
~~~java
public Bus(){
    super("소방차");
    System.out.println("Bus의 기본생성자입니다.");
}
~~~

super 키워드는 자식에서 부모의 메소드나, 필드를 사용할

## 참고사이트
-[프로그래머스 자바입문 -상속](https://programmers.co.kr/learn/courses/5/lessons/186)
-[프로그래머스 자바입문 -super와 부모생성자](https://programmers.co.kr/learn/courses/5/lessons/192)