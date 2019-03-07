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
- 책 hellcoding 그림으로보는 알고리

## int 배열 정렬 필요하면 어떻게하겠나?


참고  
- []()

## garbage collector가 어떻게 작동하는지? 정리대상클래스를 어떻게 판별하는지

참고  
- []()

## @Transactional 어노테이션과 propagation


참고  
- []()

## DB transaction isolation level


참고  
- []()

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
    - ex) ID와 password에 or 1=1-- 입
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

## CSRF 무엇이고 csrf token 작동원리가 무엇인지
- CSRF 공격(Cross Site Request Forgery)은 웹 어플리케이션 취약점 중 하나로 인터넷 사용자(희생자)가 자신의 
의지와는 무관하게 공격자가 의도한 행위(수정,삭제,등록 등)를 특정 웹사이트르 요청하게 만드는 공격
- 


참고  
- [CSRF](https://itstory.tk/entry/CSRF-%EA%B3%B5%EA%B2%A9%EC%9D%B4%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-CSRF-%EB%B0%A9%EC%96%B4-%EB%B0%A9%EB%B2%95)

## 회원가입 구현시 id,pw 를 어떻게 저장하는지


참고  
- []()

## 위키 같은 서비스 구현시 A와 B가 동시에 다른 내용으로 저장하는것을 방지하기 위해 어떻게 하겠나(html hidden)


참고  
- []()

## 테스트코드 작성 철학? 이 무엇인가


참고  
- []()

## javascript ES5, ES6 차이


참고  
- []()




## 출처
- [하이퍼커넥트 면접 후기](https://nsh330.tistory.com/entry/%ED%95%98%EC%9D%B4%ED%8D%BC%EC%BB%A4%EB%84%A5%ED%8A%B8-%EB%A9%B4%EC%A0%91-%ED%9B%84%EA%B8%B0)
