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
static {
    unitCircle = new HashSet<Point>();
    unitCircle.add(new Point(1,0));
    unitCircle.add(new Point(0,1));
    unitCircle.add(new Point(-1,0));
    unitCircle.add(new Point(0,-1));
}

public static boolean onUnitCircle(Point p){
    return unitCircle.contains(p);
}
~~~
- 그러나 값 컴포넌트를 추가하지 않는 일반적인 방법으로 Point 서브 클래스를 만든다고 해보면 얼마나 많은 인스턴스가 생성되었는지 기억하는 생성자를 갖는것이다.
~~~java
public class CounterPoint extends Point{
    private static final AtomicInteger counter = new AtomicInteger();
    
    public CounterPoint(int x, int y){
        super(x,y);
        counter.incrementAndGet();
    }
    
    public int numberCreated() { return counter.get(); }
}
~~~
- 특정 타입의 어떤 메소드건 자신의 서브 타입에서도 똑같이 동작하기 위해서 그타입의 어떤 중요한 속성도 자신의 서브타입을 위해 갖고있어야된다고 
 **리스코프의 대체원칙(Liskov substitution principle**에 나와있음
- CounterPoint  인스턴스를 onUnitCircle 메소드 인자로 전달한다고 가정
- 만일 Point 클래스에서 getClass 형태의 equals 메소드를 사용한다면, CounterPoint 인스턴스인 x와 y 관계없이 onUnitCircle 메소드에서 false를 반환할것임
    - 그 이유는 onUnitCircle 메소드에서 사용하는 HashSet과 같은 컬렉션에서 저장된 객체인지 여부를 확인하기 위해 equals 메소드를 사용하는데 CounterPoint 인스턴스는 어떤 Point 인스턴스와도 같지않기 때문이다.
- Point 클래스에서 instanceof 형태의 equals 메소드를 사용한다면, CounterPoint 인스턴스를 인자로 주어도 onUnitCircle 메소드는 잘 동작 할 것이다.

- 상속(inheritance)보다는 컴포지션(composition)을 사용하자는 권고를 따르는 것 
- ColorPoint 를 Point 서브 클래스로 만드는대신 private Point 필드와 public 뷰 메소드를 ColorPoint 클래스에 추가하는것
- 뷰 메소드는 ColorPoint 인스턴스와 같은 좌표 위치를 갖는 Point 인스턴스를 반환한다.

~~~java
// equals 계약을 어기지 않으면서 값 컴포넌트를 추가한다.
public class ColorPoint{
    private final Point point;
    private final Color color;
    
    public ColorPoint(int x, int y, Color color){
        if(color ==null)
            throw new NullPointerException();
         point = new Point(x,y);
         this.color = color;
    }
    
    /**
     * ColorPoint 인스턴스와 같은 좌표를 갖는 Point 인스턴스를 반환한다,
     */
     public Point asPoint(){
        return point;
     }
     
     @Override
     public boolean equals(Object o){
        if(!(o instanceof ColorPoint))
            return false;
         ColorPoint cp = (ColorPoint) o;
         return cp.point.equals(point) && cp.color.equals(color);   
     }
     
     ..// 나머지 코드는 생략
}
~~~
- 인스턴스 생성이 가능한 클래스로 부터 상속받아 값 컴포넌트를 추가하고 잇는 클래스들이 실제로 자바 플랫폼에 있다.
- 추상(abstract) 클래스의 서브 클래스에서는 equals 계약을 위배하지 않고 컴포넌트를 추가 할 수잇다는것에 주목
 - 예를 들어 값 컴포넌트가 없는 Shape 라는 추상클래스가 잇고 그것을 서브 클래스로 Circle, Rectangle 이 있을 수 있음
    - 이 경우 위와같은 문제점 생기지 않음 수퍼클래스의 인스턴스를 직접 생성할수 없기때문
 - 일관성(Consistency) 만일 두객체가 동일하다면, 하나가 변경되지 않는 한 항상 동일함을 유지해야 한다는것이 equals 네번째 조항
    - 즉 가변 객체들은 시점이 달라져도 다른 객체들과 동일 할 수 있지만 불변객체들은 그럴 수 없음
    - 불변 클래스로 만들어야 된다면 동일한 객체들은 꾸준히 동일함을 유지하고 동일하지 않는 객체들은 줄곧 동일하지 않음을 유지하도록 equals 메소드르 작성
- 클래스가 불변이건 아니건 신뢰할 수 없는 자원에 의존하는 equals 메소드를 작성하지말자
- Null 아님 메소드가 구현하는 동등관계 마지막 요구사항 모든 객체는 null과 같지 않아야 한다는 의미

~~~java
@Override public boolean equals(Object o){
    if(o == null)
      return false;
}
~~~
- 위와 같은 검사 불필요함 equals 메소드에서는 우선 비교에 적합한 타입으로 인자를 반환해야 하며 어차피 null이 걸러짐
    - 타입을 반환하는 이유는 객체를 비교할때 그인자가 참조하는 객체의 메소드를 호출하거나 필드를 접근 할 수 있어야 하기 때문
    - 전달된 인자의 타입이 Object dml 수퍼 클래스 타입으로 자동업캐스팅 되므로 equals 메소드 내부에서 이 인자를 올바르게 사용하려면 적합한 서브클래스 타입으로 변환(다운캐스팅) 해야함

~~~java
@Override
public boolean equals(Object o){
    if(!(o instanceof MyType))
        return false;
    myType mt = (MyType) o;
    ...
}
~~~
- 타입 검사를 하지 않았는데 잘못된 타입의 인자가 equals 메소드로 전달되면 ClassCastException 예외가 발생
- equals 계약을 위반하게됨
- instanceof 연산자로 검사하면, 두번째 피연산자(오른쪽) 타입과 상관없이 피연산자(왼쪽) null인 경우 false 가 반환된다.
- null 이 인자로 전달되면 false 가 반환되므로 따로 null을 검사 할 필요가 없음

## 양질의 equals 메소드를 만드는법
1. 객체의 값을 비교 할 필요 없고 참조만으로 같은 객체인지 비교 가능하다면 == 연산자를 사용하자. 그리고 같다면 true 를 반환함
이것은 코드 성능을 최적화 하는 것에 불과하지만, 만일 비교하는 비용이 엄청 많이 든다면 그러게할 가치가 있음

2.instanceof 연산자를 사용해서 전달된 인자가 올바른 타입인지 확인하자.
- 만일 그렇지 않다면 false 를 반환한다. 대개의 경우 올바른 타입이란 호출된 equals 메소드가 정의된 클래스를 말하지만 그 클래스가 구현하는 인터페이스도 올바른 타입이 될수 있다.
- 따라서 어떤 인터페이스를 구현하는 클래스들이 여러개 잇고 각 클래스들은 equals 계약에 맞게 equals 메소드를 오버라이딩 하고 있다 그 인터페이스도 올바른 타입이 될수 있음
- 인터페이스를 구현하는 클래스들이 여러개 있고 각클래스들은 equals 계약에 맞게 equals 메소드를 오버라이딩 하고있다면 그 인터페이스 타입으로 각 클래스들의 객체를 비교 할 수 있음
    - ex) Set,List,Map, Map.Entry 같은 컬렉션 인터페이스가 그런형태
    
3. 안자 타입을 올바른 타입으로 변환한다. 이런 타입 변환은 instanceof 검사후 행하게 되므로 예외가 생기지않고 안전하게 처리된다.

4. 클래스의 중요한(곡 비교해야 하는)필드 각각에 대해서는 인자로 전달된 객체의 필드와 현재 객체의 필드가 모두 같은지 빠트리지 말고 비교한다.
 - 모두 같다면 true, 그렇지 않다면 false 를 반환
 - 만일 앞의 2번에서 타입검사시 비교할 객체의 타입이 인터페이스일때는 그 인터페이스의 메소드를 통해서 인자의 필드를 접근하며, 비교할 객체의 타입이 클래스라면 필드를 바로 접근 할 수 있다.
 - 물론 해당 필드의 접근 범위가 어떻게 지정되었는가에 따라 다름 
 
 ~~~java
 (field == o.field || (field ! =null && field.equals(o.field))
 ~~~
 - 이런 경우에는 필드의 표준형식(canonical form)을 유지하여 equals 메소드에서 그것을 기준으로 필드 값의 동일 여부를 비교한다면 비용이 적게 드는 비교를 할 수 있음
 - 이 기법은 불변 클래스 객체를 비교할때 가장적합하다.
 - 만약 객체의 값이 변경 될 수 있다면 표준 형식을 최신으로 유지해야한다.
 - equals 메소드의 성능은 비교하는 필드의 순서에 영향을 받을 수 있다. 
    - 성능을 가장 좋게 하려면 다를 가능성이 많거나 비교비용이 적게드는 필드부터 먼저 비교
    - 객체의 동기화(synchronizeation)에 사용되는 락(lock) 필드처럼 객체 자신이 갖지 않는 필드를 비교하면 안된다.
    - 비교를 해야하는 중요필드의 값을 연산처리하여 얻어지는 파생 필드는 비교할 필요 없다.
    - 그러나 파생 필드가 그 객체 전체를 요약해서 나타내는 역할 이라면 각 필드의 값을 비교하는것보다 파생 필드를 비교하는것이 성능 향상에 도움이 될것이다.
    - 다각형(Polygon) 클래스가 있고 두 개의 다각형 객체들의 면적을 파생필드로 갖고 비교한다면 각 다각형의 변과 꼭지점을 모두 비교 할 필요가 없음

5. equals 메소드를 작성한 후에 과연 그메소드가 대칭적이며 이행적이고, 일관성 있는지를 확인함.
- 단위 테스트 코드를 작성하여 그런 속성들이 지켜지는지 검사한다.
- 만일 지켜지지 않는다면 equals 메소드를 수정한다. 물론 다른 두가지 속성(재귀성과 null이 아님 도 만족해야함)

유의할 사항
- equals 메소드를 오버라이드할때 hashCode 메소드도 항상 오버라이드 한다.
- 너무 똑똑한척 하지마라 단순히 필드 값이 같은지 검사하는 것이라면 equals 계약 준수가 어렵지 ㅇ낳다.
    - 하지만 너무 지나치게 동일여부를 비교하려면 문제가 생기기 쉽다.
    - 앨리어싱 (두개 이상의 객체 참조가 동일한 객체를 가리키는것을 앨리어싱이라고함) 된 객체들의 비교까지도 고려하는것은 좋은생각이 아님 
 - equals 메소드 인자 타입을 Object 대신 다른 타입으로 바꾸지 말자.
   
~~~java
public boolean equals(MyClass o){
 ...
}
~~~
- 여기서는 메소드 인자 Object 타입인 Object.equals를 오버라이드(override) 한것이 아니라 오버로드(overload) 하고 있다는 것이 문제이다.
- 두 메소드의 반환타입이 같다면 평범한 equals 메소드에 부가하여 또다른 equals(인자의 개수나 타입이 다른) 메소드를  정의(오버로드)해도 문제없다.
- 그러나 여기서는 그래야할 이유가없음 오버라이딩 해야할것을 잘못해서 오버로딩 한것이기 떄문에
- @Override 주석을 일관되게 사용하면 이런 실수를 막아줌 다음과 같은 equals 메소드는  컴파일에러 생길것이고 그이유는 에러메세지에서 잘알려줌
~~~
@Override public boolean equals(MyClass o){
....
}
~~~

## equals 메소드 오버라이드할떄는 hashCode 메소드도 항상 같이 오버라이드하자
- **equals 메소드를 오버라이드 하는 모든 클래스에서는 반드시 hashCode 메소드도 오버라이드 해야함**
- 그렇지 않으면 Object.hashCode 메소드의 보편적 계약을 위반하게 되므로 HashMap,HashSet,Hashtable 을 포함하는 모든 해시 기반의 컬렉션들과 
 우리 클래스를 같이 사용할 경우 올바르게 동작하지 않을 것이다.
 
자바 API 문서의 Object.hashCode 명세

- 어플리케이션 실행중에 같은 객체에 대해 한번 이상 호출되더라도 hashCode 메소드는 같은 정수를 일관성 있게 반환해야 한다.
- equals(Object) 메소드 호출 결과 두객체가 동일하다면, 두 객체 각각에 대해 hashCode 메소드를 호출 했을때 같은 정수 값이 나와야 한다.
- equals(Object) 메소드 호출 결과 두 객체가 다르다고 해서 두객체 각각에 대해 hashCode 메소드를 호출했을때 반드시 다른 정수 값이 나올 필요는 없음
    - 그러나 같이 않은 객체들에 대해 hashCode 메소드에서 서로다른 정수값을 반환하면, 이 메소드를 사용하는 해시컬렉션들(HashMap,HashSet,Hashtable)
    등이 성능을 향상 시킬수 있음을 알아야 한다.

- hashCode 의 오버라이딩에 실패 했을떄 위배되는 주요 조항은 두번째 것으로써 동일한 객체들은 같은 해시코드를 값을 가져야 한다는것
~~~java
public final class PhoneNumber{
 private final short areaCode;
 private final short prefix;
 private final short lineNumber;
 
 public PhoneNumber(int areaCode, int prefix, int lineNumber){
    rangeCheck(areaCode, 999 , "area code");
    rangeCheck(prefix, 999 , "prefix");
    rangeCheck(lineNumber, 9999, "line number");
    this.areaCode = (short) areaCode;
    this.prefix = (short) prefix;
    this.lineNumber = (short) lineNumber;
 
 }
 
 private static void rangeCheck(int arg, int max, String name){
     if(arg < 0 || arg > max)
     throw new IllegalArgumentException(name + ": "+arg);
 }
 
 @Override
 public boolean equals(Object o){
  if(o == this)
  return true;
  if(!(o instanceof PhoneNumber))
  return false;
  PhoneNumber pn = (PhoneNumber)o;
  return pn.lineNumber = lineNumber
   && pn.prefix=prefix
   && pn.areaCode = areaCode;
 }
  
  // 계약 깨짐 - hashCode 메소드가 없다!
  ...// 나머지 코드 생략

}
~~~

- 이클래스를 HashMap 과 함께 사용한다고 하면
~~~java
Map<PhoneNumber, String> m = new HashMap<PhoneNumber,String>();
m.put(new PhoneNumber(707,867, 5309),"jenny");
~~~
- m.get(new PhoneNumber(707,867, 5309)) 를 호출하면 "jenny"가 반환될것같지만 실제로는 null
- 두개의 인스턴스가 개입되어있음을 주목 하나는 HashMap 에 삽입할때 키로 사용되었고 첫번째와 동일한 값을 갖는 두번째 인스턴스는 검색 키에 사용되었음
- PhoneNumber 클래스에서 hashCode 메소드를 오버라이드 하지않아서 두 인스턴스가 서로다른 해시 코드 값을 갖게 된 것임
- 따라서 get 메소드에서는 put 메소드에 저장한 인스턴스의 것과 다른 해시 버킷(bucket)에서 PhoneNumber 인스턴스를 찾는다.
- 심지어 두 인스턴스가 같은 버킷에 놓이더라도 get 메소드는 null 을 반환하는 것임 
- HashMap 은 각 항목과 연관된 해시 코드를 저장하는 최적화 코드를 갖고 있어 해시코드값이 일치하지 않으면 객체의 동일 여부를 확인하지 않기 떄문이다.

- hoshCode 메소드 추가하면 해결됨 하지만 밑에처럼 절대 이렇게 사용하면안됨 
~~~java
@Override public int hashCode(){ return 42; }
~~~

- 동일한 객체들이 같은 해시ㅗ드를 갖게 되므로 이 메소드는 적법하다.
- 하지만 모든 객체가 다 똑같은 해시코드를 갖게 되는 최악의 메소드
- 모든 객체들은 같은 버킷에 위치하므로 이 객체들은 저장하는 해시 컬렉션(HashMap, HashSet, Hashtable)에서는 링크 리스트(linked list)를 다시 생성한다.
    - 결국 프로그램은 객체수에 선형적으로 비례하여 느리게 실행될 수 밖에 없음 큰해시 컬렉션들의 경우 동작이 되는지 의심될정도로 심각한 성능차이
- 좋은 해시 메소드는 동일하지 않은 객체들에 대해 서로 다른 해시 코드를 만
   
- 해쉬코드 겨의 균일하게 분포하는 방법
1. 예를 들어 17과 같이 0이 아닌 어떤 상수 값을 result 라는 int 변수에 저장한다.
2. 우리 객체의 각 주요필드(equals 메소드에서 비교하는)f에 대해 다음을 수행한다.

a. 각 필드에 대한 int 타입의 해시 코드 c를 다음과같이 산출한다.
1. 필드 f가 boolean 타입이면 (f ? 1 : 0)
2. 필드 f가 byte, char, short, int 타입이면, (int) f
3. 필드 f가 long 타입이면 (int) (f ^(f >>>32))
4. 필드 f가 float 타입이면 Float.floatToIntBits(f).
5. 필드 f가 double 타입이면, Double.doubleToLongBits(f)를 실행한후 반한된 long 타입의 값을 앞의 2.a.3 처럼 처리해서 해시코드 값을 구한다
6. 필드 f가 객체 참조일 경우는 현재(equals 메소드가 호출된)객체의 equals 메소드에서 그 필드를 비교하기 위해 f가 참조하는 객체의 equals 메소드를 재귀적으로 호출한다.
    - 그러면 그 객체의 필드에 대해 hashCode 메소드도 재귀적으로 호출된다. 
    - 만일 더 복잡한 비교가 필요하다면 필드의 표준 형식을 만들어 처리하고 그 표준형식에 대해 hashCode 메소드를 호출한다.
    - 만일 필드 f의 값이 null 이면 0을 반환한다.(또다른 어떤 상수 값도 가능하지만 관례적으로 0을 사용한다.)
7. 필드 f가 배열이라면 배열의 각 요소의 별개의 필드처럼 처리한다. 즉 위의 규칙들을 적용하여 처리해야할 요소 각각의 해시코드 값을 산출
    - 바로 2.b 수식을 이용하여 그값들의 합을 구한다
    - 만일 배열 필드의 모든 요소를 처리해야된다면 자바 1.5 버전에 추가되어 오버로딩된 Arrays.hashCode 메소드들 중 하나를 사용할 수있다.

b. 앞의 2.a 단계에서 구한 해시코드 c를 result에 합계한다.
    - result = 31 * result + c;
3. result를 반환한다.
4. hashCode 메소드 작성이 끝나면 동일한 인스턴스들이 같은 값의 해시코드를 갖는지 검토하자
    - 단위 테스트를 작성하여 잘 실행 되는지 검증, 만일 동일한 인스턴스들이 서로 다른 해시코드 값을 갖는다면 그 이유를 찾아 문제를 해결
    
~~~java
@Override public int hashCode(){
    int result=17;
    result = 31 * result + areaCode;
    result = 31 * result + prefix;
    result = 31 * result + lineNumber;
    return result;
}
~~~    
- 이 메소드에서 PhoneNumber 인스턴스 세가지 중요 필드만으로 간단히 연산한 결과를 반환하므로 동일한 PhoneNumber 인스턴스들은 똑같은 해시 코드 값을 갖는다.
- PhoneNumber 에 맞게 완벽하게 구현된 hashCode로써 자바 플랫폼 라이브러리의 것과 동등
- 이 메소드는 간단하고 실행속도가 빠르다. 그리고 동일하지 않은 phoneNumber 인스턴스들을 서로 다른 해시 버킷에 잘 분산시킨다. 

- 불변이면서 해시 코드 연산 비용이 중요한 클래스라면 해시 값이 매번 필요할떄마다 계산하는것 보다는 객체 내부에 해시코드를 저장하는 것을 고려해볼수있음
- 그런 타입의 객체들을 해시키로 사용할거라면 인스턴스가 생성될때 해시코드를 산출해야함
- 그렇지 않다면 hashCode 메소드가 최초 호출될때 늦은 초기화(Lazy initialization) 할 수있음

~~~java
// Lazy initialization 하면서 객체에 해시코드를 저장
private volatile int hashCode;

@Override
public int hashCode(){
 int result = hashCode;
 if( result == 0){
    result = 17;
    result = 31 * result + areaCode;
    result = 31 * result + prefix;
    result = 31 * result + lineNumber;
    hoshCode = result;
 }
 return result;
}
~~~
- 해시코드를 산출할때는 성능 항상을 이유로 객체의 중요 부분을 제외시키지말자.
    - 그로 인해 해시메소드의 실행속도는 더빨라질수 있지만 품질 저하로 인해 너무 느려 사용 불가능한 정도까지 해시 테이블들의 성능을 떨어 트릴수있음
## 참고
- 책 이펙티브자바 