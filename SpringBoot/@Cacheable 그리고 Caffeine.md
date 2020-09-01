## 현재 버전 기준 스프링 부트 캐시 종류
- Generic

- JCache (JSR-107) (EhCache 3, Hazelcast, Infinispan, and others)

- EhCache 2.x

- Hazelcast

- Infinispan

- Couchbase

- Redis

- Caffeine

Simple


## 더이상 Springboot cache 중에 guava 사용하지 않는다
Java8+ 기준으로는 guava 를 사용하지않고 Caffeine 이라는 라이브러리를 사용


[Guava cache remove Java8+버젼](https://github.com/spring-projects/spring-framework/commit/2bf9bc312ed1721b5978f88861c29cffc9ea407c)




Caffeine -> Guava에 대한 지원을 대체하는 Guava 캐시의 Java 8 재작성
https://www.javadevjournal.com/spring-boot/spring-boot-with-caffeine-cache/




## 참고사이트
- [boot-features-caching-provider-generic](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-caching-provider-generic
)
- [why did spring framework deprecate](https://stackoverflow.com/questions/44175085/why-did-spring-framework-deprecate-the-use-of-guava-cache)
