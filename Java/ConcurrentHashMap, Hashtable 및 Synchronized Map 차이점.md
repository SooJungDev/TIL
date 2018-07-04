
## ConcurrentHashMap, Hashtable 및 Synchronized Map 차이점
세가지 컬렉션 클래스는 모두 스레드에 안정성이 있음. 
스레드의 안정성을 달성하는 방법에는 차이점이있음. 

Hashtable은 스레드 안전성을 달성하기 위해 동기화 된 메서드를 사용하는 JDK1.1자체의 레거시 클래스임. 
Hashtable의 모든 메소드는 동기화되어 스레드 수가 증가하면 경쟁으로 인해 상당히 느려짐.      
Synchronized Map은 Hashtable과 크게 다르지 않음. 
둘의 차이점음 Collection.synchronizedMap()을 사용하여 맵을 래핑하여 동기화 된 버전으로 만들수있음. 


ConcurrentHashMap은 동시 사용(즉, 둘 이상의 스레드를) 위해 특별히 설계되어있음. 
기본적으로 16에 스레드가 외부 동기화 없이 Map에서 읽고 쓸 수 있음 또한 확장성이 뛰어남. 
ConcurrentHashMap은 Hashtable 및 SynchronizedMap과 달리 전체 Map을 잠그지 않고도 세그먼트로 나누고 락을 수행함


## ConcurrentHashMap과 CopyOnWriteArrayList가 필요한 이유?
동기화 된 콜렉션 클래스 ,Hashtable과 Vector및 동기화 된 래퍼클래스인 Collection.synchronizedMap() 및 Collection.synchronizedList()는 
기본적으로 thread에 안전한 map 및 list 구현을 제공합니다. 그러나 여러가지 요소로 인해 동시 처리가 많은 응용 프로그램에서 사용하기는 부적합함. 
예를 들어 단일 컬렉션 전체 잠금은 확장성에 장애가 되며 반복 실행중에 상당한 시간동안 Collection을 잠그면  ConcurrentModificationException이
발생하지 않을 수도 있음. 

ConcurrentHashMap및 CopyOnWriteArrayList 구현은 스레드 안전성을 유지하면서 훨씬 높은 동시성을 제공하며 호출자에게 약속할때 절충안을 제공함
ConcurrentHashMap및 CopyOnWriteArrayList는 HashMap또는 ArrayList를 사용할 수 있는 모든곳에서 유용하지는 않지만 특정 일반적인 상황을 최적화 할수 있도록 설계되어있음. 
많은 동시 응용프로그램을 사용한다면 큰 이점을 볼수있다.  

## ConcurrentHashMap와 Hashtable의 차이점
Hashtable과 ConcurrentHashMap의 차이점은 멀티 스레드 환경에서 사용 할 수 있지만 ,반복을 위해서는 더 긴 기간동안 잠궈야 하기때문에 Hashtable 메모리의 상당한 성능 저하를 초래함. 
  

ConcurrentHashMap은 세그먼테이션 개념을 도입했기 때문에 스레드 안전을 제공하기 위해 특정 부분만 잠겨 있기 때문에 다른 사용자가 반복을 기다리지 않고도 맵에 액세스 할수 있음.    
요약하면 ConcurrentHashMap은 맵의 특정 부분만 잠그고 Hashtable은 반복을 수행하는 동안 전체 맵을 잠군다.

