## 정리하는 이유
- websocket 관련 부하테스트를 찾아보다가 Artillery 라는 테스트 도구를 알게됨
- 자료가 많이 없어서 정리해둠

## artillery 이란?
- Artillery 는 현대적이고 강력하며 사용하기 쉬운 성능 테스트 도구
- 높은 부하에서도 성능과 탄력성을 유지하느 확장 가능한 애플리케이션을 제공하는데 사용
- 두가지 유형의 성능 테스트 실행 가능함
  - 부하테스트
  - 시스템이 예상대로 작동하는지 확인하는 테스트(시나리오 기반)
  
## artillery 특징
- 시나리오로 복잡한 사용자 동작 애물레이션 가능
- 하나의 패키지에서 기능 및 부하테스트
- HTTP, Socket.io, Websocket 은 기본으로 지원되며 플러그인( gRPC , Kafka , Kinesis , SQL , Lambda )을 통해 사용할 수 있는 다른 많은 프로토콜을 지원)
- 대규모로 테스트 실행.. 등등등

## artillery 설치
- npm 을 통해 설치 아래와 같은 명령어로 설치한다.
~~~
npm install -g artillery@latest
~~~

- 설치 확인
~~~
artillery dino
~~~

- 터미널에 아래와 같이 나옴
~~~
< Artillery! >
 ------------
          \
           \
            __
           / _)
    .-^^^-/ /
 __/       /
<__.|_|-|_|
~~~

## 테스트 스크립트
- YAML 파일 주로 두개의 부분으로 구성됨 config, scenarios

config 
- 일반적으로 대상 테스트중인 시스템의 호스트 이름 또는 IP 주소
- 로드 진행 및 HTTP 응답 시간 초과 또는 Socket.io 전송 옵션과 같은 프로토콜별 설정을 정의함
- 플러그인과 사용자 정의 JS코드를 로드하고 구성하는 데에도 사용

아래와 같은 속성이 있음
- target : 테스트 중인 애플리케이션의 URI HTTP 애플리케이션의 경우 모든 요청에 대한 기본 URL(예: http://myapp.staging.local). WebSocket 서버의 경우 서버(예: ws://127.0.0.1) 의 호스트 이름(및 선택적으로 포트)
- environments : 환경별 구성 지정
- phases : 테스트 기간 및 요청 빈도 지정
- payload : CSV 파일에서 변수 가져오기에 사용
- variables : 변수를 외부 CSV 파일에서 로드하는 대신 인라인으로 설정
- defaults : 모든 HTTP 요청에 적용할 기본 헤더를 설정
- plugins : 기능을 추가하고 Artillery의 내장 기능을 확장하도록 플러그인 구성
- processor : 시나리오 실행 중 특정 지점에서 호출될 사용자 정의 JS 코드 로드
- tls : Artillery가 자체 서명 인증서를 처리하는 방법 구성
- timeout : 서버가 응답을 시작할 때까지 기다리는 시간(초)입니다 (응답 헤더를 보내고 응답 본문 시작). 기본값은10
- ensure : 지연 또는 오류율에 대한 성공 조건을 설정합니다. CI/CD 파이프라인에서 성능 테스트를 실행하는 데 유용

### Load Phase
- 로드 단계는 Artillery가 지정된 기간에 새로운 가상 사용자(VU)를 생성하는 방법을 정의
- 예를 들어 일반적인 성능 테스트는 완만한 워밍업 단계를 거친 후 램프업 단계를 거쳐 일정시간 동안 최대 부하로 마무리됨

### 예제
- 5분 동안 1초마다 50명의 Vuser를 생성
~~~
config:
  target: "https://staging.example.com"
  phases:
    - duration: 300
      arrivalRate: 50
~~~

- 5분 동안 매초 10명의 Vuser를 생성하며 주어진 시간에 최대 50명의 동시 가상 사용자를 생성
~~~
config:
  target: "https://staging.example.com"
  phases:
    - duration: 300
      arrivalRate: 10
      maxVusers: 50
~~~

- 2분 동안 Vuser의 도착률을 10명에서 50명으로 늘립
~~~
config:
  target: "https://staging.example.com"
  phases:
    - duration: 60
      arrivalCount: 20
~~~

- 60초 동안 20명의 Vuser를 생성합니다(약 3초마다 1명의 Vuser)
~~~
config:
  target: "https://staging.example.com"
  phases:
    - duration: 60
      arrivalCount: 20
~~~


~~~
config:
  target: "https://example.com/api"
  phases:
    - duration: 60
      arrivalRate: 5
      name: Warm up
    - duration: 120
      arrivalRate: 5
      rampTo: 50
      name: Ramp up load
    - duration: 600
      arrivalRate: 50
      name: Sustained load
~~~
- Warm up : 60초 동안 1초마다 5명의 Vuser 를 백엔드로 보냄
- Ramp up load : 5명의 Vuser 로 시작하여 다음 2분동안 매초 더 많은 사용자를 보내며 단계가 끝나면 50명의 가상 사용자를 정점으로 함
- Sustained load : 다음 10분 동안 초당 50명의 Vuser 가 지속적으로 급등하는 시뮬레이션, 이단계는 더 오랜 기간 동안 시스템의 지속 가능성을 확인하기 위한것

### 그 밖에도 많은 속성이 있으므로 아래 페이지를 참고해서 사용
- [Artillery Test Scripts](https://www.artillery.io/docs/guides/guides/test-script-reference)

## 테스트 정의
- 테스트 정의는 YAML 파일로 작성됨
- artillery CLI는 그 테스트 정의를 실행하는 데 사용됨
- 테스트 정의는 하나 이상의 시나리오(scenarios)와 테스트(config) 구성으로 구성됨
~~~
config:
  target: https://artillery.io
  phases:
    - duration: 60
      arrivalRate: 10
    - duration: 600
      arrivalRate: 100
scenarios:
  - flow:
      - get:
          url: "/"
      - get:
          url: "/docs"
~~~

## websocket 관련 테스트
- think : WebSocket 서버에 연결하고 자체적으로 아무 것도 보내지 않고 수신 대기하는 클라이언트를 에뮬레이트 하는 데 사용
~~~
config:
  target: "ws://localhost:15300/api/v1"
  phases:
    - duration: 20
      arrivalRate: 10

scenarios:
  - engine: ws
    name: Test dashboard
    flow:
      - connect: "{{ target }}/dashboard/"
      - think: 600 # 10m 동안 아무것도 하지 않고 끊는다.

~~~

### 디버깅
- DEBUG=ws 
- 웹소켓 서버로 전송된 데이터 및 테스트 실행 중에 발생한 오류에 대한 세부 정보를 보려면 테스트를 실행할때 설정
~~~
DEBUG=ws artillery run my-script.yaml
~~~

- [Testing WebSockets](https://www.artillery.io/docs/guides/guides/ws-reference)



## 참고사이트
- [artillery 공식문서](https://www.artillery.io/docs)