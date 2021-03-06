## equals 오버라이드시 주의점은 무엇인가  (hashcode)
equals() 와 hashcode()
- equals는 두객체의 내용이 같은지 동등성(equality)를 비교하는 연산자
    - 객체 비교시 equals() 를 오버라이딩 해 올바른 결과를 돌려 줄수있도록 해야함
- hashCode는 두객체가 같은 객체인지, 동일성(identity)를 비교하는 연산
    - HashMap, HashSet, HashTable 과 같은 객체들은 사용하는 경우 객체의 의미상 동등성을 비교를 위해 hashCode()를 호출함
    
equals를 재정의 할 때의 일반 규약은 다음과 같음
- 반사성
    - null이 아닌 참조 x가 있을때, x.equals(x)는 true를 반환한다.
        - 즉 자기자신과 자기자신을 비교 할때 True가 나와야합니다.
- 대칭성
    - null이 아닌 참조 x와 y가 있을때, x.equals(y)는 y.equals(x)가 true일때만 true를 반환한다.
        - 순서가 뒤바뀌어도 결과는 같아야합니다.
- 추이성
    - null이 아닌 참조 x,y,z 가 있을때 x.equals(y)가 참이고 y.equals(z)가 참이면, x.equals(z)도 참이어야 한다.
- 일관성
    - null이 아닌 참조 x와 y가 있을때 equals를 통해 비교되는 정보에 변화가 없다면, 몇번 반복하더라도 같은 결과를 유지해야한다.
    
equals 재정의시 지켜야 할 점 요약
1. == 연산자를 사용하여 equals의 인자가 자기 자시인지 검사하라.
- == 연산자로 자기자신임이 판단되서 바로 리턴해버리면, 뒤의 과정을 거칠 필요가없어서 컴퓨터 자원이 절약됨

2. instanceof 연산자를 사용하여 인자의 자료형이 정확한지 검사
- instanceof에서 false가 나온 객체가 equals에서 true가 나올 수 있을까요?. 자료 객체검사를 꼭해준다.

3. equals의 인자를 정확한 자료형으로 변환하라.
- 형변환을 정확하게 해야지 앞서 했던 instanceof의 의미가 있습니다.

4. "중요" 필드 각각이 매개변수로 주어진 객체의 해당 필드와 일치하는지 검사한다.
- 파라미터로 넘겨온 객체가 가지고있는 필드값과, 현 클래스의 필드값이 일치하는지 검사합시다.

5. equals 메소드 구현을 끝냈다면 대칭성, 추이성, 일관성의 세 속성이 만족되는지 검토하라.

6. equals를 재정의 할때는 hashCode도 재정의하라.
    - 대칭성에 위배되는 경우가 있기때

7. equals 메소드의 매개변수를 Object에서 다른것으로 바꾸지마라


참고
- [Java 의 equlas 과 hashCode, 동등성과 동일성](https://blog.weirdx.io/post/3113)
- [이펙티브 자바 규칙 8 - equals 재정의 / 오버라이드 방법](https://meaownworld.tistory.com/80)

## Deadlock 이 무엇이고 발생시 어떻게 처리해야되나
Deadlock 이란?
- 스레드 하나가 특정 락(Lock)을 놓지 않고 계속 잡고 있으면 그 락을 확보하려는 다른 스레드는 락이 풀릴때까지 기다릴수 밖에없음
- Deadlock은 Thread가 두개의 락을 회득하려 하는 코드에서 나타난다.
- 데드락은 동기화 블럭이 잘못 사용되는 경우에 종종발생

Deadlock 예방하기(java)
- Lock이 발생하는 순서를정 해놓는다
- 오픈호출 : 락을 전혀 확보하지 않은상태에서 메소드 호출하는것이 좋은데 이것은 오픈호출(락을 확보하지 않은상태에서 메소드를 호출해놓는것)
- 락의 시간제한: 암묵전인 락 synchronized 말고 락 시간을 제한할수있는 Lock 클래스의 tryLock 메소드 사용
암묵적인 락은 락을 확보할때까지 기다리지만 Lock 클래스등 명시적인 락은 일정 시간을 정해두고 그시간동안 락을 확보하지 않으면 tryLock 메소드가 오류를 발생시키도록 할수있음
- Thread dump 떠서 확인

DeadLock 처리
- 교착 상태 예방 및 회피: 교착상태가 되지 않도록 보장하기 위해 교착상태를 예방하거나 회피하는 프로토콜을 이용하는방법
- 교착 상태 탐지 및 회복: 교착상태가 되도록 회복시키는방법
- 교착 상태 무시: 대부분의 시스템은 교착상태가 잘 발생하지 않으며, 교착 상태예방, 회피, 탐지, 복구하는것은 비용이 많이든다.

참고
- [Deadlock이 뭐지? (Java Thread와 Deadlock에 대한 고찰)](http://icednut.github.io/2016/08/06/20160806-about_deadlock/)

## boxing, unboxing 무엇인지, 자동으로 언제 일어나는지?
- 기본형(primitive type): byte,short,int,long,float,double,char,boolean,void
- 래퍼클래스(wrapper class):Byte,Short,Integer,Long,Float,Double,Character,Boolean,Void

- Boxing: 프리미티브 타입(기초자료형)의 값을 래퍼 객체로 만드는것
- Unboxing: 래퍼 객체를 프리미티브 타입의 값으로 만드는것
~~~java
Integer integerA = new Integer(1000); //Boxing
int num = integerA.intValue(); //Unboxing
~~~

~~~java
Integer obj =61; //AutoBoxing
Integer obj2=new Integer(69);
int num1 = obj2; // AutoUnBoxing
~~~
- 숫자 61을 Integer 객체에 넣기 위해서는 (Boxing) new Integer(61)과 같이 객체를 생성해야 하지만 위와 같이 대입하면     
AutoBoxing이 자동으로 진행된다
- Integer 객체에 있는 int값을 가져오기위해서는 (UnBoxing) obj2.intValue() 메소드를 사용하여 가져와야하지만 위와 같이  
int형 변수에 Integer 객체를 대입하면 자동으로 UnBoxing이 진행된다

- AutoBoxing과 AutoUnBoxing은 단지 기본형 타입과 상응하는 Wrapper class에만 일어난다 다른 경우에는 컴파일 에러가 발생
 - Integer는 intValue(), Double은 doubleValue등만 AutoBoxing과 AutoUnBoxing이 발생한다
 
~~~java
Double obj=3.14; 
int num1 = obj.intValue(); //(O)
int num1 = obj; //(X)
~~~

참고
- [[JAVA] Wrapper class 란? 그리고 AutoBoxing](https://hyeonstorage.tistory.com/168)
- [자동 Boxing과 자동 Unboxing](https://zzdd1558.tistory.com/58)

## mutable, immutable
- mutable : 객체 내의 특정 요소를 변경 할 수 있는 객체를 mutable 객체 ex) List,ArrayList,HashMap등
- immutable: 반대로 객체내의 특정 요소의 값을 변경 할 수 없는 객체들을 말한다 ex)String, integer,Double등등

immutable이란?
- 생성 후 변경 불가한 객체. 그래서 immutable에는 set 메소드가 없음, 멤버변수 변경할수 없음, 리턴타입이 void도 없다.
- Immutable을 쓰면 멀티스레드 환경에서 좀더 신뢰 할 수 있는 코드를 만들어내기 쉽다.

대표적인 Immutable 클래스
- String, Boolean, Integer, Float, Long 등등
- heap 영역에서의 변경 불가
- String a="a"; a="b"; 재할당은 가능
- 이는 a가 reference 하고있는 heap 영역의 객체가 바뀌는것이지 heap 영역에 있는 값이 바뀌는 것은아님

String vs StringBuffer
- String은 immutable
- StringBuffer는 mutable
- StringBuffer가 String에 비해서 훨씬 빠르다라는 애기
    - 객체를 새로 생성할 필요가 없기때문임
 
Immutable의 유용성
- 멀티스레드 환경에서 하나의 객체에 접근하는데 각각의 스레드끼리는 영향받으면 안되는 경우가 있습니다.   
그럴때 한번 만들어진 객체의 값이 변하지 않는다는게 보장되면 편함
    

참고
- [MUTABLE 과 IMMUTABLE 객체](https://yangyag.tistory.com/63)
- [자바에서 Immutable이 뭔가요?](https://hashcode.co.kr/questions/727/%EC%9E%90%EB%B0%94%EC%97%90%EC%84%9C-immutable%EC%9D%B4-%EB%AD%94%EA%B0%80%EC%9A%94)

## generic erasure
- 자바에서는 제너릭 클래스를 인스턴화 할때 해당 타입을 지워버린다.
- 그 타입은 컴파일시 까지만 존재하고, 컴파일된 바이트코드에서는 어떠한 타입파라미터 정보를 찾아볼수 없음

~~~java
List<Integer> numbers = new ArrayList<>();
~~~

바이트 코드레벨에서는 new ArrayList(); 와 동일
- 하위 호완성을 매우 중요시해서 제네릭을 사용하지 않았던 레거시코드에서도 돌아가기위해서

제네릭
- 제네릭은 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에서 컴파일 시 타입 체크를 해주는 기능
- 즉 클래스 내부에서 사용할 데이터 타입을 나중에 인스턴스 생성할때 확정하는 것을 제너릭

제네릭의 장점
1. 타입 안정성을 제공한다.
2. 타입체크와 형변환을 생략 할 수 있으므로 코드가 간결해진다.

참고
- [[Java] 제네릭](https://devbox.tistory.com/entry/Java-%EC%A0%9C%EB%84%A4%EB%A6%AD)
- [자바의 제네릭 (Generic)](http://wonwoo.ml/index.php/post/1743)

## encoding이 뭔지?, java의 한글은 몇바이트인지 , string에 문자열은 어떻게 저장되나?
- 부호화나 인코딩은 정보의 형태나 형식을 변환하는 처리나 처리방식이다.
- 문자 인코딩은 문자들의 집합을 부호화하는 방법이다.

자바 내부에서 기본적으로 문자에대해서 유니코드 사용, 영문이든 한글이든 2바이트
UTF-8에선 한글 3바이트

String 은 두가지 생성방식이 있고 각각의 차이점이 존재함
1. new 연산자를 이용한 방식
    - Heap 영역에 존재하게 됨
2. 리터럴을 이용한 방식
    - String constant pool 이라는 영역에 존재 자바 7이전 perm 영역, 이후는 Heap 영역에 존재하기됨
    - Heap 영역에 존재하게 된 이유는 runtime에 사이즈가 확장되지 않기 때문에 
    - OutOfMemoryException을 발생시킬 수있음
String은 값이 변경 될때마 새로운 객체를 생성하기 된다.이런작업을 수행하면서 메모리를 많이사용하게됨


참고
- [인코딩 wiki](https://ko.wikipedia.org/wiki/%EC%9D%B8%EC%BD%94%EB%94%A9)
- [자바에서 인코딩(encoding)](https://linuxism.ustd.ip.or.kr/588)
- [java String 객체에 대한 이해 (StringBuilder,StringBuffer 등을 사용해야 하는 이유 및 기타 String의 특징)](https://opentutorials.org/module/1226/8020)
- [Java String 의 메모리에 대한 고찰](https://medium.com/@joongwon/string-%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0-57af94cbb6bc)

## static
static 은 보통 변수나 메소드 앞에 static 키워드를 붙여서 사용하게된다

static 변수
- 변수에 static 키워드를 붙이면 자바는 메모리 할당을 딱 한번만 하게 되어 메모리 사용에 이점을 볼 수 있게 된다.
- 공유의 개념이 있음 
- static 으로 설정하면 같은곳의 메모리 주소만 바라보기 때문에 static 변수의 값을 공유하게 되는것
- 보통 변수의 static 키워드는 프로그래밍 시 메모리의 효율보다는 두번째 처럼 공유하기 위한 용도로 훨씬 더많이 사용하게 된다.

static method
~~~java
public class Counter{
    static int count = 0;
    
    Counter(){
        this.count++;
    }
    
    public static int getCount(){
        return count;
    }
    
    public static void main(String[] args){
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        
        System.out.println(Couter.getCount());
    }
}
~~~
- getCount() 라는 static 메소드가 추가되었다. main 메소드에서 getCount()메소드는  
Counter.getCount() 와 같이 클래스를 통해 호출 할 수 있게 된다.

- static 메소드안에서 인스턴스 변수 접근이 불가능하다. 위 예에서 count는 static 변수이기 때문에
static 메소드에서 접근이 가능한 것이다.

- 보통 static 메소드는 유틸리티 성 메소드를 작성 할때 많이사용됨

싱글톤 패턴(singleton patter)
- 디자인 패턴중 하나
- 싱글톤은 단 하나의 객체만 생성하게 강제하는 패턴이다. 
- 즉 클래스를 통해 생성 할 수 있는 객체는 Only One, 즉 한개만 되도록 만드는것이 싱글톤이다.

~~~java
class Singleton{
    private static Singleton one;
    private Singleton(){}
    
    public static Singleton getInstance(){
        if(one==null){
            one = new Singleton();
        }
        return one;
    }
}

public class SingletonTest {
    public static void main(String[] args){
        Singleton singleton1 = Singleton.getInstatance();
        Singleton singleton2 = Singleton.getInstatance();
        System.out.println(singleton1 == singleton2); // true 출력
    }
}
~~~
- 최초 getInstance 가 호출되면 one이 null이므로 new에 의해서 객체가 생성된다.
- 이렇게 한번 생성되면 one은 static 변수이기 때문에 그 이후에는 null 이 아니게 된다.
- 그런 후 다시 getInstance를 호출하면 one이 null이 아니므로 이미 만들어진 싱글톤 객체인 one을 항상 리턴하게 된다.


참고  
- [정적 변수와 메소드 (static)](https://wikidocs.net/228)

## stack, heap 영역별로 무엇이 저장되는지?
Stack 영역
- 지역 변수와 매개변수가 저장된다.
- 참조 변수에 저장되는 메모리 주소는 스택영역에 저장되지만, 그주소가 가리키는 메모리는 모두 힙영역에 저장

Heap 영역
- new 명령어를 통해 생성된 인스턴스 변수가 저장됨
- 참조 변수들은 실행 될때마다 많은 데이터들을 스택 메모리 영역에 뒀다 뺐다 하는게 매우 비효율적이다
    - 힙영역에 그 주소가 가리키는 값이 저장되고(변수가 가리키고 있는값), 스택 메모리에는 간단하게 주소만 저장


참고  
- [자바 메모리 관리 - 스택 & 힙](https://yaboong.github.io/java/2018/05/26/java-memory-management/)
- [자바 메모리 구조](https://wanzargen.tistory.com/17)
## syhchronized{} 에 붙을때와 메소드에 붙을때 차이
- 코드를 메소드 수준에서 관리 할 수 있다
- 코드를 블록 수준에서 관리할 수있다.
    - 동기화 영역을 최소화 할때 synchronized 블록을 사용

참고  
- [synchronized 키워드의 의미를 알고 메소드와 블록에 붙을 때의 차이를 알아야 한다.](https://opentutorials.org/module/1226/8028)

## hashtable 에서 get,put의 시간복잡도
get : 평균 O(1), 최악 : O(n)
put:   평균 O(1), 최악 : O(n)
remove:  평균 O(1), 최악 : O(n)

참고  
- 책 hello coding 그림으로보는 알고리즘

## int 배열 정렬 필요하면 어떻게하겠나?
java.util.Arrays
- Arrays 클래스에는 배열을 다루기 위한 다양한 메소드가 포함되어 있다.
- Arrays 클래스의 모든 메소드 클래스 메소드(static method)이므로, 객체를 생성하지 않고도 바로 사용 가능
- 이 클래스는 java.util 패키지에 포함되므로, 반드시 import 문으로 java.util 패키지를 불러오고 나서 사

- java.util.Arrays 클래스 사용
Arrays.sort() 함수를 사용해서 정렬

- 오름차순
~~~java
int[] arr ={21,4,19,31,16};
Arrays.sort(arr);
~~~

- 내림차순
~~~java
Integer[] arr2={4,2,1,5,3};
Arrays.sort(arr2, Collections.reverseOrder());
~~~
- Comparator 로는 Collections class의 reverseOrder() 함수를 이용
- 내림차순 적용


참고  
- [java 배열 정렬](https://jamesdreaming.tistory.com/162)
- [Arrays 클래스](http://tcpschool.com/java/java_api_arrays)

## garbage collector가 어떻게 작동하는지? 정리대상클래스를 어떻게 판별하는지
Java의 가비지 컬렉터(Garbage Collector)는 크게 다음 2가지 작업을 수행
1. 힙(heap) 내 객체중에 가비지(garbage)를 찾아낸다.
2. 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다.

GC와 Reachability
- Java GC는 객체가 가비지인지 판별하기 위해서 reachability라는 개념을 사용
- 어떤 객체에 유효한 참조가 있으면 reachable로 없으면 unreachable로 구별
- unreachable 객체를 가비지로 간주해 GC를 수행한다.
- 한 객체는 여러 다른 객체를 참조하고, 참조된 다른 객체들도 또다른 객체들을 참조 할 수 있으므로 객체들은 참조 사슬을 이룬다.
- 이런 상황에서 유효한 참조 여부를 파악하려면 항상 유효현 최초의 참조 
    - 이를 객체의 참조 root set이라고 한다.
    
- java GC는 GC 대상 객체를 찾고, 대상 객체를 처리(finalization)하고, 할당된 메모리 회수하는 작업으로 구성된다.
- 애플리케이션은 사용자 코드에서 객체의 reachability를 조절하여 java GC에 일부 관여 할수있다.
- 객체의 reachability를 조절하기 위해서 java.lang.ref 패키지의 SoftReference, WeakReference,
PhantomReference, ReferenceQueue 등을 사용한다.


참고  
- [Java Reference와 GC](https://d2.naver.com/helloworld/329631)

## @Transactional 어노테이션과 propagation
- 스프링 트랜잭션은 개발자의 편리하기위해 트랜잭션 관리를 위한 일관된 추상화를 제공
Transaction의 기본설정은 확인되지 않는 Exception(Runtime Exception)발생시 RollBack 하도록 설정


propagation (전파옵션)
- REQUIRED: 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성
- REQUIRES_NEW: 부모 트랜잭션을 무시하고, 무조건 새로운 트랜잭션 생성
- SUPPORT: 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 nontransactionally로 실행
- MANDATORY: 부모 트랜잭션 내에서 실행되며, 부모 트랜잭션이 없을 경우 예외가 발생
- NOT_SUPPORT: nontransactionally로 실행하며 부모 트랜잭션 내에서 실행될 경우 일시정지
- NEVER: nontransactionally로 실행되며 부모 트랜잭션이 존재한다면 예외가 발생
- NESTED: 해당 메서드가 부모 트랜잭션에서 진행될 경우 별개로 커밋되거나 롤백될 수 있음.
    - 둘러싼 트랜잭션이 없을 경우 REQUIRED와 동일하게 작동


참고  
- [@Transactional](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)
- [Spring Transaction 옵션](https://taetaetae.github.io/2016/10/08/20161008/)
- [Spring-framework-reference](https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/transaction.html)
- [Spring Transaction Management Over Multiple Threads](https://dzone.com/articles/spring-transaction-management-over-multiple-thread-1)

## DB transaction isolation level
트랜잭션이 보장해야되는 ACID
- Atomicity(원자성): 한 트랜잭션 내에서 실행한 작업들은 하나의 작업으로 간주한다. 모두 성공 또는 모두 실패되어야됨
- Consistency(일관성): 모든 트랜잭션은 일관성 있는 데이터베이스 상태를 유지한다. 이를테면 DB에서 정한 무결성 조건을 항상 만족
- Isolation(격리성): 동시에 실행되는 트랜잭션들이 서로 영항을 미치지 않도록 격리해야한다.
- Durability(지속성): 트랜잭션을 성공적으로 마치면 그 결과가 항상 저장되어야 한다.

Isolation에 대한 이슈가 있음
- 격리성을 완벽히 보장하기 위해 모든 트랜잭션을 순차적으로 실행한다면 동시성 처리 이슈가 발생한다.
- 반대로 동시성을 높이기 위해 여러 트랜잭션은 병렬처리하게 되면 데이터의 무결성이 깨질 수 있다.

격리성 관련 문제점  
1.Dirty Read
- 한 트랜잭션(T1)이 데이터에 접근하여 값을 'A'에서 'B'로 변경 했고, 아직 커밋하지 않았을때
- 다른 트랜잭션(T2)이 해당 데이터를 read 하면?
    - T2가 읽은 데이터는 B가 될 것임.
    - 하지만 T1이 최종 커밋을 하지 않고 종료된다면 T2 데이터는 꼬이게됨
    
2.Non-Repeatable Read
- 한 트랜잭션(T1)이 데이터를 Read 하고 있다. 이때 다른 트랜잭션(T2)가 데이터를 접근하여 값을 변경 또는 데이터를 삭제하고 커밋을 하면?
    - 그 이후 T1이 다시 해당데이터를 Read 하고자 하면 변경된 데이터 혹은 사라진 데이터를 찾게된다.

3.Phantom Read
- 트랜잭션(T1) 중에 특정 조건으로 데이터를 검색하여 결과를 얻었다.
- 이때 다른 트랜잭션(T2)가 접근해 해당 조건의 데이터 일부를 삭제 또는 추가 했을때  
아직 끝나지 않은 T1이 다시 한번 해당 조건으로 데이터를 조회하면 T2에서 추가/삭제 된 데이터가 함께 조회/누락된다.
- T2가 롤백을 하면 데이터가 꼬인다


4단계 격리수준을 나누었음.  
내려갈수록 격리 수준이 높아져서 언급된 이슈는 적게 발생하지만, 동시처리 성능은 떨어진다.
참고 트랜잭션이 발생하면 Rock이 걸리는데,  
SELECT 시 에는 공유 락,  
CREATE/INSERT/DELETE 시에는 배타적 락이 걸림

1.Read Uncommitted
- 한 트랜잭션에서 커밋하지 않은 데이터에 다른 트랜잭션이 접근 가능하다. 즉 커밋하지 않은 데이터를 읽을 수 있다.
    - 이 수준은 당연히 위에서 언급한 모든 문제에 대해 발생가능성이 존재한다.
    - 대신, 동시 처리 성능은 가장 높음
- 발생 문제점: Dirty Read, Non-Repeatable Read, Phantom Read

2.Read Committed
- 커밋이 완료된 데이터만 읽을 수 있다.
 - Dirty Read가 발생할 여지는 없으나, Read Uncommitted 수준보다 동시 처리 성능은 떨어진다.
 - 대신 Non-Repeatable Read 및 Phantom Read는 발생 가능하다.
 - 데이터베이스들은 보통 Read Committed를 디폴트 수준으로 지정한다.
- 발생 문제점:Non-Repeatable Read, Phantom Read

3.Repeatable Read
- 트랜잭션 내에서 한번 조회한 데이터를 반복해서 조회해도 같은데이터가 조회된다.
 - 이는 개별 데이터 이슈인 Dirty Read나 Non-Repeatable Read는 발생하지 않지만,  
   결과 집합 자체가 달라지는 Phantom Read는 발생가능하다.
- 발생 문제점: Phantom Read

4.Serializable
가장 엄격한 격리수준
- 위 3가지 문제점을 모두 커버 가능하다.
- 하지만 동시 처리 성능은 급격히 떨어 질 수 있다.

참고  
- [트랜잭션, 트랜잭션 격리수준(Isolation Level)](https://feco.tistory.com/45)
- [Transaction Isolation Levels in DBMS](https://www.geeksforgeeks.org/transaction-isolation-levels-dbms/)

## sql injection이 무엇이고 어떻게 방지하냐
- SQL Injection이란 Web hacking 기법중 하나이다. 
- 웹 애플리케이션의 뒷단에 있는 Database에 질(쿼리 보내는 것)하는 과정에서 
일반적인 값외에 악의적인 의도를 갖는 구문을 삽입하여 공격자가 원하는 SQL 쿼리문을 실행하는기법이다.
주로 사용자가 입력한 데이터를 제대로 필터링,이스케이핑 하지 못했을 경우에 발생한다.
- 요즘의 거의 모든 데이터베이스 엔진은 유저 입력이 의도치 않은 동작을 하는 것을 방지하기 위해 escape 함수와
prepared statement 제공한다.

SQL injection 공격의 종류에는 크게 3가지
- 인증우회 (AB: Auth Bypass)
    - 보통 아이디와 패스워드를 입력하는 로그인 페이지를 타겟으로 행해지는 공격이다.
    - SQL 쿼리문의 true/false의 논리적 연산 오류를 이용하여 로그인 인증 쿼리문이 무조건 true의 결과값이 나오게 하여 인증을 무력화
    - ex) ID와 password에 or 1=1-- 입력
- 데이터 노출(DD: Data Disclosure)
    - 타켓 시스템의 주요 데이터 절취를 목적으로 하는 방식
    - 악의적인 구문을 삽입하여 에러를 유발시키는 
    -  오류가 나타난다면 그것을 가지고 데이터 베이스 구조를 유추할수있음
    - 그렇기 때문에 오류 페이지 또는 오류 메세지가 출력되서는 안됨
- 원격명령 실행(RCE: Remote Command Excute)

방어방법
1. 대부분의 SQL Injection의 경우 입력받을떄 특수문자 여부를 검사하여 방어한다.
    - 아이디와 패스워드를 입력받는 입력에 특수문자가 포함되어 있는지 검증 로직을 추가하여 특수문자가 입력된 경우 해당 요청을 막아 낼 수 있다.
2. SQL 서버에 오류가 발생 했을 시 해당하는 에러 메시지를 표시해선 안된다.
- 원본의 테이블에는 접근권한을 높이고 일반 사용자는 view로만 접근하게 되면 원본테이블이 공격에 의해 노출되는 정도를 줄일 수 있음
3. Statement 대신 preparestatement를 사용하자
- mysql의 Preparedstatement 구문을 사용하면 특수문를 자동으로 escaping 해준다.
클라이언트 측 자바스크립트 폼 입력값을 우선 검증함.
- 하지만 자바스크립트는 공격자가 꺼버리면 그만이기떄문에 서버측에서 입력값을 한번 더 필터한다.
4. hash function을 사용하자.
- 사용자 입력값을 데이터베이스 그대로 저장하고 사용하지 말아야 한다. 특히 패스워드의 경우가 그렇다.
무조건 SHA-256 이상의 보안성을 갖는 해시함수로 해싱한 뒤 저장해야 한다.


참고  
- [SQL Injection이란 무엇인가](https://asfirstalways.tistory.com/360)

## Blind SQL Injection이란?
- 임의의 SQL 구문을 삽입하여 인가되지 않은 데이터를 열람할 수 있는 공격방법
- 일반적인 SQL Injection과 동일
- 다만 일반적인 SQL Injection의 경우에는 조작된 쿼리를 입력해 한번에 원하는 데이터를 얻을 수있지만  
Blind SQL Injection은 쿼리 결과에 따른 서버의 참과 거짓 반응을 통해 공격을 수행한다.
- 즉 쿼리에 대한 결과가 참일때와 거짓일때 서버의 반응이 나타나야 하며 이를 구분 할 수 있어야 한다.
~~~
select * from users where id='admin' and ascii(substr(select pw from user where id ='admin',1,1))<100 -- and pw='입력받은 pw'
~~~
위와 같은 방법을 반복하면 pw의 첫글자를 알아 낼 수 있으며 더나아가 pw의 전체를 알아낼수 있음

참고
- [Blind SQL Injection ](https://4rgos.tistory.com/9?category=694721)

## CSRF 무엇이고 csrf token 작동원리가 무엇인지
- CSRF 공격(Cross Site Request Forgery)은 웹 어플리케이션 취약점 중 하나로 인터넷 사용자(희생자)가 자신의 
의지와는 무관하게 공격자가 의도한 행위(수정,삭제,등록 등)를 특정 웹사이트르 요청하게 만드는 공격

CSRF 공격이 이뤄지려면 다음조건이 만족되어야함
- 위조 요청을 전송하는 서비스(페이스북)에 희생자가 로그인 상태
- 희생자가 해커가 만든 피싱 사이트에 접속

CSRF 방어 기법
- Referrer 검증
- Security Token 사용

CSRF Token
- 우선 사용자의 세션의 난수 값을 저장하고, 사용자의 요청마다 해당 난수 값을 포함 시켜 전송시킵니다.
- 이후 Back-end단에서 요청을 받을 때마다 세션이 저장된 토큰값 요청파라미터에 전달되는 토큰 값이 일치하는지 검증하는 방법입니다.
- 이 방법도 XSS 취약점 있다면 CSRF 공격에 취약해집니다.

참고  
- [CSRF](https://itstory.tk/entry/CSRF-%EA%B3%B5%EA%B2%A9%EC%9D%B4%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-CSRF-%EB%B0%A9%EC%96%B4-%EB%B0%A9%EB%B2%95)

## XSS와 CSRF 차이점
- XSS는 공격대상이 Client이고 CSRF는 Server이다.
- XSS는 사이트변조나 백도어를 통해 클라이언트에 대한 악성공격을 한다.
- CSRF는 요청을 위조하여 사용자의 권한을 이용해 서버에 대한 악성공격을한다.

참고
- [XSS와 CSRF에 대하여](https://glasgowkiss.tistory.com/16)

## 회원가입 구현시 id,pw 를 어떻게 저장하는지
- Spring security 사용
- bcrypt 로 암호화해서 디비에 저장

스프링 시큐리티란?
- 스프링 기반의 어플리케이션의 보안(인증과 권한)을 담당하는 프레임워크이다.
- 만약 스프링 시큐리티를 사용하지 않는다면, 자체적으로 세션을 체크하고, redirect 등을 해야함
- 스프링 시큐리티는 보안과 관련해서 체계적으로 많은 옵션들로 이를 지원해줌

보안관련 용어
- Principal(접근주체): 보호된 대상에 접근하는 유저
- Authenticate(인증) : 현재 유저가 누군지 확인(로그인)
    - 어플리케이션의 작업을 수행 할 수 있는 주체임을 증명
- Authorize(인가): 현재 유저가 어떤 서비스, 페이지에 접근 할 수 있는 권한이 있는지 검사
- 권한: 인증된 주체가 어플리케이션 동작을 수행 할 수 있도록 허락되었는지를 결정
    - 권한 승인이 필요한 부분으로 접근하려면 인증 과정을 통해 주체가 증명 되어야만 한다.
    - 권한 부여에도 두가지 영역이 존재하는데, 웹 요청 권한, 메소드 호출 및 도메인 인스턴스에 대한 접근 권한부여

참고  
- [안전한 패스워드 저장](https://d2.naver.com/helloworld/318732)
- [pring security 파헤치기](https://sjh836.tistory.com/165)

## 위키 같은 서비스 구현시 A와 B가 동시에 다른 내용으로 저장하는것을 방지하기 위해 어떻게 하겠나
- 공유 객체를 둬서 먼저 접근한 A에게 권한을 주고 B는 접근하지 못하도록??...



## 테스트코드 작성 철학? 이 무엇인가
- Test-Driven Development
- Lean 소프트웨어 개발론의 핵심철학중 하나 "결함은 발견 즉시 해결"이다. 린 개발은 이것의 실천법으로 테스트 주도 개발을 제시한다
- 반복테스트를 이용한 소프트웨어 개발법
- 작은 단위의 테스트케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 소프트웨어를 구현
- 목표는 "Clean code that works" 이다. 
- 오직 자동화된 테스트가 실패할 경우에만 새로운 코드를 작성한다.
- 중복을 제거함
- 테스트가 주된목적이 아닌 "문제를 먼저 정의하고, 문제의 해답을 찾아가는 과정"
- 내가 지금 만들어야 할 것이 무엇인지 우선적으로 명확하게 정의한 후에 그 내용을 테스트로 표현하는게 TDD의 근본 취지
    - BDD(Behavior Driven Development) 
 - 개발자가 아닌 사용자 요구사항 관점 명세정의
 
TDD 개발법
아래 단계의 반복으로 진행 
RED: 실패하는 작은 테스트 케이스를 작성한다. 처음에는 컴파일조차 안될 수있다.
GREEN: 테스트를 통과하는 코드를 작성한다.
REFACTORING: 테스트를 통과하기 위해 만든 코드의 모든 중복을 제거하고, 불명확한 것을 명확히 한다.
 - 같은 코드가 세번이상 사용되면
 
코드구조
Given 에는 "어떤 상황"이 들어간다.
When에는 "어떻게 동작한다"가 들어간다.
Then에는 "동작한 결과가 어떠해야 한다"가 들어간다.

TDD 장점
- 개발자의 방향을 잃지 않게 유지
- 품질 높은 소프트웨어 모듈 보유: 간결한 코드 유지 가능
- 자동화된 단위 테스트 케이스 소유: 개발자가 필요한 시점에 언제든 수행 할 수 있으며, 시스템의 이상 유무를 바로 확인 할 수 있다.
- 사용설명서 & 의사소통의 수단: 작성한 테스트 케이스는 제품 코드 사용 설명서이자 동시에 다른 개발자와 소통하는 커뮤니케이션 통로가됨
- 설계 개선: TDD는 주기를 짧게 설정하도록 권한다. 개발자는 성취감을 자주 느낄 수 있다.


참고  
- [TDD란?](https://m.blog.naver.com/PostView.nhn?blogId=hannaj92&logNo=220665822239&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

## javascript ES5, ES6 차이점
ES5(2009) 
ES6(ES2015)
- 호이스팅이 사라진것 같은 효과
- 함수 단위 스코프에서 블록단위 스코프로 변경
- this를 동적으로 바인딩 하지 않는 화살표 함수
- 모듈화 지원
- 콜백 지옥에서 구원해줄 Promise
    - ES8(ES2017) asyn, await
- Default, Rest 파라미터
- 해체 할당, Spread 연산자
- 템플릿 리터렁
- .....등등등

참고  
- [ES6?! ES2015?! ECMAScript란 도대체 무엇인가?](https://luckydavekim.github.io/web/2018/02/07/what-is-the-ecmascript/)

## UDP와 TCP란 무엇인가?
- 모두 전송계층에서 동작하는 프로토콜
- 두 프로토콜은 모두 패킷을 한 컴퓨터에서 다른 컴퓨터로 전달해주는 IP프로토콜을 기반으로 구현되었지만 서로다른 특징을 가지고 있음

TCP(Transmission Control Protocol)
- 신뢰성있는 데이터 전송을 지원하는 연결 지향형 프로토콜
- TCP 는 패킷을 성공적으로 전송하면 ACK라는 신호를 받음
- 만일 ACK 신호가 제 시간에 도작하지 않으면 Timeout이 발생하여, 패킷 손실이 발생한 패킷에 대해 다시 전송해줌
- 연결 지향적인 TCP는 3-way handshaking 이라는 과정을 통해 연결 후 통신
- 또한 흐름제어와 혼잡제어를 지원하며 데이터의 순서를 보장함
- UDP에 비해 속도가 느림 
- 대부분 HTTP 통신, 이메일, 파일전송에 사용됨

UDP
- 전송계층의 비연결형 프로토콜
- TCP와 달리 연결 설정없음 혼잡제어하지 않기때문에 tcp보다 빠른장점
- 데이터 전송에 대한 보장을 하지 않기떄문에 패킷 손실이 발생 할 수 있음
- UDP는 이런 특성떄문에 DNS, 멀티미디어에서 사용
- UDP 헤더에 있는 Checksum 필드를 통해 최소한의 오류는 검출함
- 최근에는 속도가 빠른 UDP에 신뢰성 있는 데이터 젖송을 추가하여 서버를 구현하기도함

- [TCP와 UDP 비교 정리](https://swalloow.tistory.com/77)

## 출처
- [하이퍼커넥트 면접 후기](https://nsh330.tistory.com/entry/%ED%95%98%EC%9D%B4%ED%8D%BC%EC%BB%A4%EB%84%A5%ED%8A%B8-%EB%A9%B4%EC%A0%91-%ED%9B%84%EA%B8%B0)
- 잡플래닛 면접 후기
