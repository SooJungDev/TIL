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
- Boxing: 프리미티브 타입의 값을 래퍼 객체로 만드는것
- Unboxing: 래퍼 객체를 프리미티브 타입의 값으로 만드는것
~~~java
Integer integerA = new Integer(1000); //Boxing
int num = integerA.intValue(); //Unboxing
~~~

~~~java
Integer integerObj = new Integer(30);
int sum = integerObj + 20;
System.out.println(sum);
~~~
reson - 위의 예제에서는 Integer 객체와 int 타입의 값을 더했다. +연산자는 프리미티브 타입의 값을 더하는 연산자이기 떄문에
Integer 객체로부터 int값이 자동으로 추출된 후에 더하기 연산이 실행됩니다.

참고
- [자동 Boxing과 자동 Unboxing](https://zzdd1558.tistory.com/58)

## mutable, immutable

## generic erasure

## encoding이 뭔지?, java의 한글은 몇바이트인지 , string에 문자열은 어떻게 저장되나?


## static

## stack, heap 영역별로 무엇이 저장되는지?

## syhchronized{} 에 붙을때와 메소드에 붙을때 차이

## hashtable 에서 get,put의 시간복잡도

## int 배열 정렬 필요하면 어떻게하겠나?

## garbage collector가 어떻게 작동하는지? 정리대상클래스를 어떻게 판별하는지

## @Transactional 어노테이션과 propagation

## DB transaction isolation level

## sql injection이 무엇이고 어떻게 방지하냐

## CSRF 무엇이고 csrf token 작동원리가 무엇인지

## 회원가입 구현시 id,pw 를 어떻게 저장하는지

## 위키 같은 서비스 구현시 A와 B가 동시에 다른 내용으로 저장하는것을 방지하기 위해 어떻게 하겠나(html hidden)

## 테스트코드 작성 철학? 이 무엇인가

## javascript ES5, ES6 차이




## 출처
- [하이퍼커넥트 면접 후기](https://nsh330.tistory.com/entry/%ED%95%98%EC%9D%B4%ED%8D%BC%EC%BB%A4%EB%84%A5%ED%8A%B8-%EB%A9%B4%EC%A0%91-%ED%9B%84%EA%B8%B0)
