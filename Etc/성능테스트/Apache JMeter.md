## Apache JMeter 
- HTTP 요청시 동시에 요청하는 테스트를 해 볼 일이 있어서 JMeter 사용 할 일이 생겨서 사용하게 되었다

## JMeter 설치 및 실행
- [Download Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi) 사이트로 들어 가서 apache-jmeter-XXX.tgz 다운
-  터미널에서 다운받은 경로로 들어가서 apache-jmeter-5.2.1/bin 밑으로 들어간다
- ./jmeter.sh 실행
 
- JMeter 를 실행하면 기본적으로 TEST plan 이 있음
- Test Plan 에 추가해야 할 것은 Thread Group 
  - JMeter 테스트는 쓰레드를 생성하여 요청을 하게 되는데, Thread Group은 쓰레드 생성 규칙에 대한 정의를 하는 공간이며, 여러 테스트에 대한 그룹 단위

Thread Group을 생성하고 스레드 생성 규칙을 추가
![스크린샷 2019-12-05 오후 2 09 53](https://user-images.githubusercontent.com/38197944/70206068-ac6c3900-1769-11ea-9431-79eede7c8a83.png)
- Test Plan 우클릭 -> add -> Thread(Users) -> Thread Group 을하여 생성한다.

![스크린샷 2019-12-05 오후 2 10 21](https://user-images.githubusercontent.com/38197944/70206084-b9892800-1769-11ea-91d7-907d86b7889e.png)
위에 속성 이해해야함 
1. **Number of Threads** : 스레드 개수, 즉 가상 유저의 수
2. **Ramp-Up Period** : 스레드 당 생성 시간 
 - 예를 들어 Number of Threads = 1000 이고, Ramp-Up = 10 일때 
 1000 명의 유저를 생성 할 때 까지 10초가 걸린다는 것, 다시말하면 1초 동안 100명의 유저가 요청한다는 뜻
 - 따라서 Ramp-Up =0 으로 설정하면 동시 접속자 수는 1000명이 됨
3. **Loop Count** : 하나의 Thread의 수행할 작업 수 
 - 예를 들어 Number of Thread - 1000이고 Loop Count = 10 일때
 1000명의 유저는 동일한 작업을 10번 수행하게 됨 따라서 총 10000번이 수행되며 , 이는 총 요청횟수로 생각하면됨

위의 속성 값을 정의할때 주의해야함!!!
- 스레드 수가 지나치게 많을 경우 CPU 과부하가 되고 그러면 올바른 테스트 결과를 얻을 수없기 때문에 위에 해당하는 값 적절히 조정해줘야함
- CPU와 메모리 상태를 확인한다

## HTTP Request 생성
- 실제로 어떤 경로에 요청을 보낼 것인지 정의해야 하는데, 이를 작성하는 곳이 HTTP Request 임
![스크린샷 2019-12-05 오후 2 27 48](https://user-images.githubusercontent.com/38197944/70206636-7e87f400-176b-11ea-97e9-a323a5937a1c.png)
- Thread Group 오른쪽 클릭 -> add -> Sampler -> HTTP Request


![스크린샷 2019-12-05 오후 2 30 30](https://user-images.githubusercontent.com/38197944/70206792-0110b380-176c-11ea-8969-2b823f71d787.png)
- 위에 해당되는 프로토콜, 도메일 이름 또는 IP 주소, 포트번호 작성, HTTP 요청 메서드와 경로를 작성해준다.

## 응답 결과 데이터 조회
- 요청에 대한 응답 결과를 분석을 하기 위해 여러가지 결과 데이터가 필요함
- 무엇을 테스트  것이냐 따라 추가하는 Listener 가 다름

Listener 를 추가하는 방법
- ThreadGroup -> add -> Listener ->  선택

1. View Results Tree : Request, Response data 등 요청에 대한 정보
2. Summary Report : 요청 결과를 파일로 저장 할 수 있음. 하단의 Save Table Data 버튼 클릭하면 됨, 데이터를 바탕으로 엑셀에서 그래프를 그릴 수 있음
3. Response Time Graph : 응답 시간 결과를 그래프로 볼 수 있음 ,settings 에서 x축 간격 좁힐 수 있음

## 테스트
- 상단에 start 버튼 누르면 Thread 생성되면서 요청하게됨
- 주의할 점은 start 하기전에 빗자루 버튼 클릭 이전 테스트 내용을 모두 지운후에 테스트
 

 
 
 ## 참고사이트
 - [Apache JMeter - HTTP, WebSocket 성능 테스트](https://victorydntmd.tistory.com/267)
