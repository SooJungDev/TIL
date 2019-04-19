## equals 메소드를 오버라이딩 할떄는 보편적 계약을 따르자
- 인스턴스의 동일여부를 판정하는 equals 소드는 오버라이딩이 간단한것 같지만 잘못되는 경우가 ㅁ낳음
    - 문제를 피하는 가장 쉬운 방법은 equals 메소드(object 것)
    - 다른 슈퍼클래스에서 object 이미 오버라이딩 한것을 오버라이드 하지 않고 상속받은 그대로 사용

- 클래스의 각 인스턴스가 본래부터 유일한경우 
    -인스턴스가 갖는 값보다는 활동하는 개체임을 나타내는것이 더중요한 스레드와 같은 클래스가 여기 해당
    - 그런 클래스들은 인스턴스가 갖는 논리적인 비교는 의미가 없음
    - Object의 equals를 그냥 사용하면됨

- 두 인스턴스가 논리적으로 같은지 검사하지 않아도 되는 클래스의 경우
    - ex) java.util.Random 클래스으ㅔ서는 두개의 Random 인스턴스가 같은 난수열을 만드는지 확인하기 위해 equals 메소드를 오버라이딩 할 수 있었을 것이었다.
    -  Object의 equals를 그냥 사용하면됨
    
- 수퍼 클래스에서 equals 메소드를 이미 오버라이딩 했고, 그메소드를 그대로 사용해도 좋은경우
    - Set 인터페이스를 구현하는 대부분의 클래스들은 AbstractSet 에 구현된 equals를 상속받아 사용함
    - List 경우는 AbstractList 에서 상속받음
    - Map AbstractMap 에서 상속받음
 
- private 이나 패키지 전용(package-private) 클래스라서 이클래스의 equals 메소드가 절대 호출되지 않아야 할 경우
    - 접근 지시자를 지정하지 않은 디폴트 패키지 접근을 말함
    - 오버라이딩 해서 호출되지 않도록 해야함
~~~java
@Override public boolean equals(Object o){
    throws new AssertionError(); // 메소드가 절대 호출되지 않는다.
}
~~~

## Object.equals 는 언제 오버라이드해야되나???

- 객체 참조만으로 인스턴스 동일 여부를 판단하는것이아니라 인스턴스가 갖는 값을 비교하여 논리적으로 같은지 판단할 필요가 있는 클래스로써  
자신의 수퍼클래스에서 equlas 메소드를 오버라이드 하지 않았을때 
- 일반적으로 value 클래스가 여기에 해당
    - 하나의 값을 나타내는 클래스 ex) Integer, Date
    - 객체 참조 여부는 중요하지 않고 객체가 갖는 값이 논리적으로 같은지가 관심사
    - equals 메소드의 오버라이딩은 프로그래머의 욕구 충족
    - Map의 키나 Set의 요소를 객체가 이미 있는지 비교하는 수단을 제공해야하기 떄문

## equals 메소드를 오버라이드 할 필요없는 클래스

- 각 값 당 최대하나의 객체만 존재하도록 인스턴스 제어를 사용하는 클래스
    - 열거형 enum
    - 그런 클래스들의 경우 논리적인 일치와 객체 참조 일치가 동일한 의미
    - Object equals 메소드가 논리적인 equals 메소드의 기능을 하는 셈

## equals 메소드를 오버라이드 할때 보편적 계약

- 재귀적이다(Reflexive)
    - null이 아닌 모든 참조 값 x에 대해, x.equals(x) 반드시 true 반환
   
    
- 대칭적이다(Symmetric)
    - null이 아닌 모든 참조 값 x와 y에 대해 y.equals(x)가 true 반환하면
    - x.equals(y) 도 반드시 true 반환해야함
    
- 이행적이다(Transitive)
    - null 이 아닌 모든 참조 값 x,y,z 에 대해 , equals 메소드에서 객체 비교시 사용하는 정보가 변경되지 않는다면
    - x.equals(y)를 여러번 호출하도록 일관성있게 true 또는 false를 반환해야함
    - null이 아닌 모든 참조값 x에 대해 x.equals(null)은 반드시 false 반환해야함
    
    
- 재귀성(Reflexivity)
    - 첫번쨰 조항에는 객체를 자기 자신과 비교하면 같아야된다고 나와있음
    - 어기기 어려움
    
- 대칭성(Symmetry)
    - 두 객체건 대칭적으로 서로 비교하면 같아야됨
     - 재귀성과 다르게 무심 어기기 쉬움

예를들어 영문 대소문자를 구분하지 않는 문자열을 구현
~~~java
// 계약 깨짐 - 대칭성에 위배됨
public final class CaseInsensitiveString{
    private final String s;
    
    public CaseInsesitiveString(String s){
        if(s == null)
            throws new NullPointerException();
         
         this.s=s;
    
    }
    
    //계약 깨짐 - 대칭성에 위배됨
    @Override
    public boolean equals(Object o){
        if(o instanceof CaseInsesitivieString)
            return s.equalsIgnoreCase((CaseInsesitiveString) 0).s);
        if(o instanseof String) //한쪽으로만 상호 운용된다.
            return s.equalsIgnoreCase((String) o);
        return false;    
    }
    ...//나머지코드 생략
}
~~~    
- equals 메소드에서 대소문자를 구분하지 않는 문자열(CaseInsensitiveString 객체)과 일반문자열(String 객체) 모두를 같이 처리하려고 한다.

~~~java
CaseInsesitiveString cis = new CaseInsensitiveString("Polish");
String s = "polish";
~~~
- cis.equals(s)는 true 반환
- 그러나 CaseInsensitiveString 의 equals 메소드는 일반 문자열 처리하고 있는 반면
- String 의 equals 메소드는 대소문자를 구분하지 않는 문자열을 인식하지 못하는 것이 문제임
- 따라서 s.equals(cis)는 false 반환 
    - 대칭성을 위반
    
~~~java
List<CaseInsensitiveString> list = new ArrayList<CaseInsensitiveString>();
list.add(cis);
~~~
- 일단 equals 계약을 어기면,우리객체와 같이 사용되는 다른 객체들이 어떻게 동작할지 알수 없음

- 문제를 해소하려면 equals 메소드로 부터 String 객체를 처리하는 코드를 제거하면됨
~~~java
@Override
public boolean equals(Object o){
    return o instanseof CaseInsensitiveString &&
      ((CaseInsensitiveString) o).s.equalsIgnoreCase(s);
}
~~~


이행성(Transitivity)
- 첫번째 객체가 두번째 객체와 동일하고 두번째 객체가 세번째 객체와 동일하다면 첫번째 객체는 세번째 객체와 같아야 함

- 기존 수퍼 클래스에 값 컴포넌트(value component) Color 객체를 추가하는 서브 클래스 생각
    - equals 메소드가 객체를 비교하는데 필요한 정보를 서브 클래스에서 추가 제공

- 정수 좌표이 점을 나타내는 불변(immutable) 클래스
~~~java
public class Point{
    private final int x;
    private final int y;
    
    public Point(int x, int y){
        this.x=x;
        this.y=y;
    }
    
    @Override
    public boolean equals(Object o){
        if(!(o instanceof Point))
            return false;
        Point p =(Point)0;
        return p.x == x && p.y === y;    
    }
    
    ...// 나머지코드 생략

}
~~~

- 점마다 색상을 추가해서 다음과 같은 서브 클래스
~~~java
public class ColorPoint extends Point{
    private final Color color;
    
    public ColorPoint(int x, int y, Color color){
        super(x,y);
        this.color = color;
    }
    
    // 나머지 코드 생략
}
~~~

~~~java
@Override
public boolean equals(Object o){
    if(!(o instanceof ColorPoint))
        return false;
    return super.equals(o) && ((ColorPoint) o).color == color;
}
~~~
- 이 메소드에 문제점 Point 객체를 ColorPoint 와 비교할때 그리고 그반대로 비교할떄 다른결과 나옴
- Point 객체를 ColorPoint 비교할때는 색상이(Color) 비교에서 빠짐 반대로 비교할떄는 항상 false 반환
    - 인자값이 틀리기 떄문에    
~~~ java
Point p = new Point(1,2);
ColorPoint cp = new ColorPoint(1,2, Color.RED);
~~~
- 이경우 p.equals(cp)는 true 반면 cp.equals(p)는 false

~~~java
// 계약 깨짐 - 이행성에 위배된다
@Override
public boolean equals(Object o){
    if(!(o instanseof Point))
        return false;
        
     // 만일 o가 Point 객체라면 Color를 빼고 비교한다.
     if(!(o instanceof ColorPoint))
        return o.equals(this);
        
      // o가 ColorPoint 객체라면, Point와 Color 모두 비교한다.
      return super.equals(o) && ((ColorPoint)o).color == color;
}
~~~
- Point 객체를 비교할때는 색상(Color)를 무시하도록 다음과 같이 ColorPoint.equals 수정하여 문제 해결할수있지만 문제가생김
    - 이행성에 대한 문제

~~~java
ColorPoint p1 = new ColorPoint(1,2,Color.RED);
Point p2 = new Point(1,2);
ColorPoint p3 = new ColorPoint(1,2,Color.BLUE);
~~~
- p1.equals(p2), p2.equals(p3) true 반환
- p1.equals(p3)는 false 반환
    - 명백한 이행성 위반
   
   
- **인스턴스 생성이 가능한 클래스의 서브 클래스 값 컴포넌트를 추가하면서 equals 계약을 지킬 수 잇는 방법은 없음**

~~~java
// 리스코프(Liskov)의 대체원칙(substitution principle)에 위배된다.
@Override public boolean equals(Object o){
    if(o == null || o.getClass() != getClass())
        return false;
     Point p = (Point) o;
     return p.x == x && p.y ==y;
}
~~~
- 위 경우 비교하는 두객체의 클래스만 같을떄만 효과 있음

- 정수의 좌표 값을 갖는 점이 단위원(Unit Circle) 상에 있는지 알려주는 메소드작성
~~~java
// 단위원 상의 모든 접믈 포함하도록 UnitCircle 초기화 한다.
private static final Set<Point> unitCicle;
~~~

## 참고
- 책 이펙티브자바 