## EHCache 설정하기
설정 순서
1. Maven Dependency 설정
2. Ehcache.xml 작성 (ehcache 설정 파일)
3. CacheManager 설정 (xml)

## ehcache.xml 작성
- default Cache 설정 반드시 구현해야 할 캐시 (직접 생성하는 캐시에 대한 기본설정)
- cache 는 하나의 캐시를 사용할때마다 구현
- name 속성은 캐시의 이름을 지정하며, 코드에서는 이 캐시의 이름을 사용하여 사용할 Cache 인스턴스를 구한다.

속성
- name : 코드에서 사용할 캐시 네임 필수로 있어야함
- maxElementsInMemory : 최대 메모리 용량 
- maxEntiresLocalHeap : 메모리에 생성 될 최대 캐시 갯수
- maxEntiresLocalDisk : 디스크에 생성될 최대 캐시 갯수
- etenral : 영속성 캐시 설정 true 이면 timeToIdleSecond, timeToLiveSecond
- timeToIdleSecond : 해당 초동안 캐시가 호출되지 않으면 삭제
- timeToLiveSeconds: 해당 초가지나면 캐시삭제
- overflowToDisk: 오버 플로우 된 항목에대해 디스크에 저장할지 여부
- diskPersistent: 캐시를 디스크에 저장하여 서버로드시 캐시를 말아둘지 설정
- diskExpiryThreadIntervalSeconds : Disk Expiry 스레드의 작업 수행 간격 설정
- memoryStoreEvictionPolicy: 캐시의 객체수가 maxEntriesLocalHeap 도달하면 객체를 추가하고 제거하는 정책 설정
    - LRU : 가장 오랫동안 호출 되지 않은 캐시를 삭제
    - LFU : 호출 빈도가 가장적은 캐시를 삭제 
    - FIFO : 캐시가 생성된 순서대로 가장 오래된 캐시를 삭제


## 참고사이트
- [EHCache 설정방법 (Spring Framework)](https://jaehun2841.github.io/2018/11/07/2018-11-04-ehcache-config-for-springframework/)
- [ehcache 캐싱하기](https://softarchitecture.tistory.com/7)