## 스프링 부트 소개
- 스프링 부트는 운영 수준의 어플리케이션을 빠르고 쉽게 만들 수 있음
- 모든 스프링 개발을 할때 빠르고 쉽게 폭넓은 설정 제공해줌
- 일일이 설정하지 않아도 컨벤션으로 설정할 수 있음 
- xml 설정 쓰지 않음, 코드 제너레이션을 하지 않는다. (더쉽고 명확, 커스텀마이징 하기 쉬움)
- spring boot는 자바 8부터 사용 가능함 

## 스프링부트 프로젝트 구조
메이븐 기본 프로젝트 구조와 동일  

- 소스 코드 (src\main\java)
- 소스 리소스 (src\main\resource)
- 테스트 코드 (src\test\java)
- 테스트 리소스 (src\test\resource)

메인 애플리케이션 위치  
- 기본 패키지
- 포함되어있는 패키지 밑에 컴포넌트 스캔을 함 

## 의존성 관리 이해 
- 스프링 부트 디펜던시 직접 관리할 필요가 없음 
- parent로 설정해줌

## 참고사이트
  - 인프런 강좌 백기선 스프링부트 개념과 활용 정리
  - [스프링 부트 소개](https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#getting-started-introducing-spring-boot)
  - https://start.spring.io/
  - [스프링부트 프로젝트 구조](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-structuring-your-code)
  - [의존성 관리 이해](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-dependency-management)