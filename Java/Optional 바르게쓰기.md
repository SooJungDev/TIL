## Optional 바르게 쓰기
- Optional 메서드가 반환할 결과값이 없음을 명백하게 표현할 필요가 있는곳에서 제한적으로 사용 할 수 있는 매커니즘을 제공하는 것이 Optional 을 만든 의도
- isPresent()-get() 대신 orElse()/orElseGet()/orElseThrow()
- orElse(new ...) 대신 orElseGet(() -> new ...)
 - orElse(...) 에서 ... 는 Optional 에 값이 있든 없든 무조건 실행된다. 
 - 따라서 ...가 새로운 객체를 생성하거나 새로운 연산을 수행하는 경우에는 orElse 대신 **orElseGet()** 을 써야됨
- 단지 값을 얻을 목적이라면 Optiona 대신 null 비교
- Option 대신 비어있는 컬렉션 반환
- Optional 을 필드로 사용 금지
- Optional 을 생성자나 메서드 인자로 사용 금지
- Optional 을 컬렉션의 원소로 사용 금지
    - 원소를 사용하지말고 원소를 꺼낼떄나 사용할때 널체크 하는것이 좋음
    - 특히 Map은 getOrDefault(), putIfAbsent(), computeIfAbsent(), computeIfPresent() 처럼 null 체크가 포함된 메서드를 제공하므로, 
        Map의 원소로 Optional을 사용하지 말고 Map이 제공하는 메서드를 활용
- of(), ofNullable() 혼동 주의
  - of()는 null 이 아님이 확실 할 때만 사용해야함
  - ofNullable() null 일 수 도 있을때만 사용해야하며, null이 아님이 확실하면 of(X)를 사용해야한다.
- Optional<T> 대신 OptionalInt, OptionalLong, OptionalDouble


## 참고사이트
- 아래글 좋은글이다!! Optional 을 사용 할 때 마다 확인해보기 익숙해지기 전까지!
- [Java Optional 바르게 쓰기](https://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/)
