## Log4j(Log for java)

### 주요 기능 및 특징

- 속도와 유연성을 고려하여 디자인 되었으며, 속도에 최적화
- 멀티스레드 환경에서 사용해도 안전
- 계층적인 로그 설정과 처리를 지원한다.
- 출력을 파일, 콘솔, java.io.OutputStream, java.io.Writer, TCP 를 사용하는 원격서버, 원격 Unix Syslog 데몬 ,
 원격 JMS 구독자, 윈도우NT EventLog, email 로 보낼수 있음
- 계층적인 6가지의 로그 메시지 레벨을 사용
  - TRACE, DEBUG, INFO, WARM, ERROR , FETAL

### Log4j 구조

- Logger(Category): Log4j 에서 지원하는 핵심클래스 , 로깅이 일어나는 부분을 그룹화해 필요한 그룹의 로그만을 출력하거나
카테고리에 우선순위를 부여함으로써 여러가지 출력방법을 지원한다. 실제 로그 기능을 수행하는 역할 담당

- Appender: 로그의 출력 위치를 지정한다. Appender로 설정 가능한 출력은 콘솔, 파일, OutputStream , java.io.Writer,
Email(SMTP), Network 등이 있다.
 - ConsoleAppender : 콘솔에 로그 메세지를 출력
 - FileAppender: 파일에 로그 메시지를 출력
 - RollingFileAppender : 로그의 크기가 지정한 용량이상이되면 다른 이름의 파일을 출력
 - DailyRollingFileAppender: 하루를 단위로 로그 메시지를 파일에 출력
 - SMTPAppender : 로그 메세지를 이메일로 보냄
 - NTEventLogAppender : 윈도우의 이벤트 로그 시스템에 기록

- Layout: 로그의 출력 포맷을 지정한다. 단순한 텍스트 출력, 포맷을 직접 지정한 패턴, HTML 문서 형식등을 사용 할 수있음
~~~
%d : 로그의 기록시간을 출력
%p : 로깅의 레벨을 출력
%F : 로깅이 발생한 프로그램의 파일명을 출력
%M : 로깅이 발생한 메소드의 이름을 출력
%ㅣ : 로깅이 발생한 호출지의 정보를 출력
%L : 로깅이 발생한 호출지의 라인수를 출력
%t : 로깅이 발생한 Thread 명을 출력
%c: 로깅이 발생한 카테고리를 출력
%C : 로깅이 발생된 클래스명 출력
%m : 로그 메세지를 출력
%n : 개행문자 출력
%r: 어플리케이션이 시작 이후부터 로깅이 발생한 시점까지 시간을 출력
%x : 로깅이 발생한 Thread 와 간련된 NDC(nested diagnostic context) 출력
    NDC란???? -> 여러소스의 인터리브된 로그 메시지를 구별하는데 도움이되는 메커니즘
%X : 로깅이 발생한 Thread MDC(mapped diagnostic context) 출력
    MDC란 ?? -> java.util.Map 형식을 이ㅛㅇ하여 클라이언트 특징적인 데이터를 저장하기 위한 메카니즘
~~~

### Log4j 로깅레벨
- **지정한것 이상의 레벨의 메세지가 출력**
- FATAL : 가장 크리티컬한 에러가 일어 났을떄 사용
- ERROR : 일반 에러가 일어 났을 때 사용
- WARN : 에러는 아니지만 주의할 필요가 있을 때 사용
- INFO : 일반 정보를 나타 낼 때 사용
- DEBUG : 일반 정보를 상세히 나타날 떄 사용
- TRACE : 경로추적을 위해 사용 

### Log4j 장단점
장점 
- 프로그램 문제파악이 용이
- 빠른 디버깅 가능
- 로그 파악 쉬움
- 로그 이력 파일, DB 등을 기록할수있음
- 효율적으로 디버깅 가능

단점
- 로그에 대한 디바이스 파일입출력으로 인해 런타임 오버헤드가 생김
- 로깅을 위한 초가 코드로 인해 전체 코드 사이즈가 증가
- 심하게 생성되는 로그는 혼란을 일으킬수 있음
- 심하게 생성되는 로그는 어플리케이션 성능에 영향을 미칠 수 있음
- 개발중간에 로깅 코드를 추가하기 어려움

## LogBack
- log4j를 토대로 새롭게 만든 Logging 라이브러리
- slf4j 를 통해 연관 라이브러리들이 다른 logging framework 를 쓰더라도 logback 으로 통합
- Logback 을 이용하여 로깅을 수행하기 위해서 필요한 주요 설정 요소 Logger, Appender, Encoder 3가지 읬음

- Logger: 실제 로깅을 수행하는 구성요소로 Level 속성을 통해서 출력할 로그의 레벨을 조절 할 수 있음
- Appender: 로그메세지가 출력될 대상을 결정하는 요소
- Encoder: Appender에 포함되어 사용자가 지정한 형식으로 표현 될 로그 메시지를 변환하는 역할을 담당하는 요소

Logback에서 설정파일을 작성하는 방법은 2가지
- XML 을 이용한 설정방법: logback.xml 설정 파일 작성후 해당 파일을 클래스패스에 위치시킴
- Groovy 언어를 이용한 설정방법: logback.groovy 로 설정 파일 작성후 해당 파일을 클래스패스에 위치시킨다.

장점
- log4j 보다 약 10배정도 빠르게 수행되도록 내부가 변경 되었으며 , 메모리 효율성 좋아짐
- log4j때부터 광범위한 테스트를 진행한 경험을 가지고 있으며, logback은 더욱 높은 레벨의 테스트를 통해 검증
- 문서화가 잘되어있음
- 설정파일을 변경 하였을 경우 , 서버 재기동 없이 변경 내용이 자동으로 갱신
- 서버 중지 없이 I/O Failure 에 대한 복구 지원
- RollingFileAppender 를 사용할 경우 자동적으로 오래된 로그를 지워주며 Rolling 백업을 처리한다.



  
    

## 참고사이트
- [Log4j 및 LogBack 정리](https://goddaehee.tistory.com/45)
- [NDC & MDC ?](http://egloos.zum.com/charmpa/v/2543451)