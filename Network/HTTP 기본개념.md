## HTTP 개요
- HTTP는 HTML 문서와 같은 리소들을 가져올 수 있도록 해주는 프로토콜임
- HTTP는 보통 브라우저의 수신자에 의해 요청이 초기화 되는것을 의미하는 웹과 클라이언트-서버 모델상에 모든 데이터 교환의 기초
- 보통 브라우저인 클라이언트에 의해 전송되는 메세지를 요청(requests)
- 그에 대해 서버에서 응답으로 전송되는 메시지 응답(response)이라고 부름

## HTTP 기반 시스템 구성요소
- HTTP는 클라이언트 -서버 프로토콜
- 요청은 하나의 개체, 사용자 에이전트(또는 그것을 대신하는 프록시)에 의해 전송됨
- 서버는 요청을 처리하고 response라고 불리는 응답 제공

Client -> Proxy -> Proxy -> Sever  
Client <- Proxy <- Proxy <- Server


## HTTP의 기초적인 측면
- HTTP은 간단하다
- HTTP은 확장 가능하다.
    - HTTP 헤더는 확장이 쉬움
- HTTP는 상태가 없지만 세션은 있음
    - HTTP 핵심은 상태가 없는것이지만 HTTP 쿠키는 상태가 있는 세션을 만들도록해줌
    - 헤더 확장성을 사용하여 동일한 컨텍스트 또는 동일한 상태를 공유하기위해 각각의 요청들에 세션을 만들도록 HTTP 쿠키가 추가됨
- HTTP와 연결
    - HTTP는 연결 될수있도록하는 근본적인 전송 프로토콜 요구 X
    - 연결이 필수는 아니지만 연결기반인 TCP 표준에 의존
    - HTTP/1.1 파이프라이닝 개념과 지속적인 견결 개념 도입
        - 이전 버전 각 요청/응답에 대해 별도의 TCP 연결을 열였음(효율적이지 못하다!!!)


## HTTP로 제어 할 수 있는것
- 캐시
- origin 제약사항을 완화하기
- 인증
- 프록시와 터널링
    - HTTP 요청은 네트워크 장벽을 가로지르기위해 프록시를 통해 나감
- 세션
    - 쿠키사용은 서버 상태를 요청과 연결하도록 해줌     
    
    
## HTTP 흐름
- 클라이언트가 서버와 통신하고자 할때, 최종서버가 됐든 중간 프록시가 됬든 다음단계 과정을 수행

1.TCP 연결을 연다. TCP 연결은 요청을 보내거나 응답을 받는데 사용됨
    - 클라이언트는 새 연결을 열거나 기존 연결을 재사용하거나, 서버에 대한 여러 TCP 연결을 열 수 있음
2.HTTP 메시지를 전송합니다. HTTP 메시지(HTTP/2 이전의)는 인간이 읽을수 있음 
    - HTTP/2에서는 메세지가 프레임속으로 캡슐화되어 직접읽는게 불가능함
~~~
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
~~~

3. 서버에 의해 전송된 응답을 읽어들임
~~~
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
~~~
4. 연결을 닫거나 다른요청들을 위해 재사용함

## HTTP 메시지
- HTTP 메시지의 두가지 타입인 요청(request)과 응답(response)은 각자의 특성있는 형식을 가지고 있음

request 구성요소
- HTTP 메소드
- 가져오려는 리소스의 경로,
- HTTP 프로토콜의 버젼
- 서버에 대한 추가 정보를 전달하는 선택적 헤더들
- POST와 같은 몇가지 메소드를 위한 전송된 리소스를 포함하는 응답의본문과 유사한 본문

response 구성요소
- HTTP 프로토콜의 버전
- 요청의 성공여부와, 그이유를 나타내는 상태코드
- 아무런 영향력이없는 상태 코드의 짧은 설명을 나타내는 상태 메시지
- 요청 헤더와 비슷한 HTTP 헤더들
- 선택 사항으로 가져온 리소스가 포함되는 본문 

## 참고사이트
- [MDN web docs HTTP](https://developer.mozilla.org/ko/docs/Web/HTTP)