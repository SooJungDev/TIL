## REST API
API
- Application Programming Interface

REST
- REpresentational State Transfer
- 인터넷 상의 시스템 간의 상호 운용성(interoperability)를 제공하는 방법중 하나
- 시스템 제각각의 독립적인 진화를 보장하 위한 방법
- REST API : REST 아키텍처 스타일을 따르는 API

REST 아키텍처 스타일
- Client-Server
- Stateless
- Cache
- Uniform Interface
- Layered System
- Code-On-Demand (optional)

독립적 진화
- 서버와 클라이언트가 각각 독립적으로 진화한다
- 서버의 기능이 변경되어도 클라이언트를 업데이트 할 필요없다
- [Rest를 만들계된 계기](https://www.infoq.com/articles/roy-fielding-on-versioning)

-> REST API 두가지를 만족해야함 : 하이퍼텍스트를 포함한 Self-descriptive 한 메세지 Uniform interface 를 통해 리소스에 접근하는 APIC 

Uniform Interface
- identification of resources
- manipulation of resources through representations
- self-descriptive messages
- hypermedia as the engine of application state(HATEOAS)

- Self-descriptive message
  - 메세지 스스로 메세지에 대한 설명이 가능해야함
  - 서버가 변해서 메세지가 변해도 클라이언트는 그 메세지를 보고 해석이 가능하다
  - 확장 가능한 커뮤니케이션
  - 메세지 자체만으로 언제든 해석이 가능하단 의미
 
- HATEOAS
   - 어떤 상태로 전이가 완료되고 나서야 그다음 전이될 수있는 상태가 결정됨 (링크는 동적으로 변경 될 수 있음)
   - 하이퍼미디어(링크)를 통해 애플리케이션 상태 변화가 가능해야함
   - 링크 정보를 동적으로 바꿀 수 있다.(Versioning 할 필요없이)
 
 Self-descriptive message 해결방법
 - 방법1: 미디어 타입을 정의하고 IANA에 등록하고 그 미디어 타입을 리소스 리턴할때 Content-Type을 사용함
 - 방법2: profile 링크 헤더를 추가한다.
    - 브라우저들이 아직 스펙 지원을 잘 하지 않음
    - 대안으로 HAL의 링크 데이터에 Profile 링크 추가
 
HATEOAS 해결 방법
- 방법1: 데이터 링크 제공 
  - 링크를 어떻게 정의 할 것인가? HAL
- 방법2: 링크 헤더나 Location을 제공

## 스프링 프로젝트 만들기
추가 할 의존성
- Web
- JPA
- HATEOAS
- REST Docs
- H2
- PostgreSQL
- Lombok

자바 버젼 11

스프링부트 핵심원리
- 의존성 설정(pom.xml)
- 자동 설정(@EnableAutoConfiguration)
- 내장 웹서버(의존성과 자동설정의 일부)
- 독립적으로 실행가능한 JAR(pom.xml의 플러그인)