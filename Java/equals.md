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
    public boolean 

}

~~~    

## 참고
- 책 이펙티브자바 