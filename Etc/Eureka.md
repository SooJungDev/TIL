## Eureka
- 정의 : Middle-tier load balancer
- 목적 
 - 로드 밸런싱과 장애복구가 가능한 Middle-tier 서비스 환경을 구성했을때 클라이언트 API Gateway 또는 다른서비스에게 가용한 
 서비스 인스턴스들의 위치 정보를 동적으로 제공 할 수 있어야 한다.
- 전통적인 로드 밸런서는 서버의 고정적인 IP 주소와 Host name 을 기준으로 동작하는 반면, AWS 와 같은 클라우드 환경에서는 
서버위치가 동적으로 변할 수 있기 때문에 로드 밸런서에 그위치를 등록하고 변경하는 과정이 훨씬 더 까다롭다
- 또한 AWS 는 middle-tier load balance를 제공하지 않는다.

## 참고사이트
- [Eureka git book](https://coe.gitbook.io/guide/service-discovery/eureka)