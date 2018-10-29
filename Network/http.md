## HTTP 특징
- 인터넷상에서 데이터를 주고받기위한 서버/클라이언트 모델을 따르는 전송프로토콜
- tcp/ip 위에서 작동
- 클라이언트에서 요청(request)를 보내면 서버는 ㅛ청을 처리해서 응답(response)
- port 80번 이용
- 비상태연결(stateless, connectless) 
- 서버에 연결하고 요청해서 응답받으면 연결을 끊어버림
- Keep Alive : 지정된 시간동안 연결을 끊지않고 요청을 계속 보냄 

## HTTPS 특징
- SSL 데이터 보안을 위해서 개발한 통신 레이어를 가지고있음
- SSL은 표현계층의 프로토콜로 응용계층 아래에 있기때문에 어떤 응용계층의 데이터라도 암호화해서 보낼수있음
- 기본 포트 번호 443
- HTTP에 비해 많이느림

## Request
~~~
Get /test/test.htm HTTP/1.1
Accept: */*
Accept-Language: ko
Accept-Encoding: gzip, deflate
If-Modified-Since: Fri, 21 Jul 2006 05:31:13 GMT
If-None-Match: "734237e186acc61:a1b"
User-Agent: Mozilla/4.0(compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; InfoPath.1)
Host: localhost
Connection: Keep-Alive



HTTP/1.1 200 OK
Server: Microsoft-IIS/5.1
X-Powered-By: ASP.NET
Date: Fri, 21 Jul 2006 05:32:01 GMT
Content-Type: text/html
Accept-Ranges: bytes
Last-Modified: Fri, 21 Jul 2006 05:31:52 GMT
ETag: "689cb7f885acc61:a1b"
Content-Length: 101
~~~

- 요청라인: GET/HTTP/1.1
    - 요청 메소드: get, post, put, delete, trace
    - 요청 url
    - http 버젼
- 요청 헤더 :  key, value 값으로 들어감
    - Accept: 클라이언트가 받을수 있는 컨텐츠 */*은 특정 유형이아닌 모든 파일형식을 지원한다는 의미
    - User-Agent: 클라이언트 소프트웨어(브라우저, os)등의 이름과 버젼
    - Host: 요청을 한 서버 host
    - Cookie : 쿠키
    - Content-Type: 메세지 바디 종류
    - Content-Length: 메세지 바디길이
    - if-Modified-Since: 페이지가 수정되었으면 최신버전 페이지 요청을 위한 필드, 만일 요청한 파일이 필드에 지정된 시간이후로 변경하지않았다면 서버로부터 데이터를 전송받지않음
    - Refer: 특정 페이지에서 링크를 클릭하여 요청을 하였을 경우에 나타남
    - Accept-Language: 클라이언트가 인식 할 수 있는 언어로 우선수위 지정이 가능함
    - Accept-Encoding: 클라이언트가 인식 할 수 있는 인코딩 
- 요청바디(entity)

## Response
~~~
HTTP/1.1 200 OK
Server: Microsoft-IIS/5.1
X-Powered-By: ASP.NET
Date: Fri, 21 Jul 2006 05:32:01 GMT
Content-Type: text/html
Accept-Ranges: bytes
Last-Modified: Fri, 21 Jul 2006 05:31:52 GMT
ETag: "689cb7f886acc61:a1b"
Content-Length: 101
~~~

- HTTP/1.1 200 ok : http 버젼과 응답 코드(200 성공)
- Server : 웹서버 정보를 나타냄
- Date : 현재날짜
- Content-Type: 요청한 파일의 MIME타입을 나타냄 
- Last-Modified: 요청한 파일의 최종 수정일을 나타냄
- Content-Length: 헤더이후 이어지는 데이터의 길이(byte 단위)
- ETag: 캐쉬 업데이트 정보를 위한 임의의 식별 숫자

상태코드
- 1xx:정보
- 2xx: 성공
  - 200: ok, 요청성공
  - 201: created 생성요청 성공
  - 202: Accepted 요청수락 (처리보장 x)
  - 204: 성공했으나 돌려줄게없음
- 3xx: 리다이렉션
  - 300: multiple choices 여러 리소스에 대한 요청 결과 목록
  - 301,302,303: Redirect 리소스 위치가 변경된 상태
  - 304: Not modified. 리소스가 수정되지 않음
- 4xx: 클라이언트 오류
    - 400: Bad Request 요청 오류
    - 401: Unauthoried: 권한 없음
    - 403: Forbidden 요청거부
    - 404: Not found 리소스가 없는상태
- 5xx: 서버오류
  - 500:Interal Server Error: 서버가 요청을 처리못함
  - 501: Not Implemented: 서버가 지원하지 않는 요청
  - 503: Service Unavailable 과부하 등으로 당장 서비스가 불가능한 상태 
        

## 참고사이트
  - [HTTP 구조](http://sjh836.tistory.com/81)
  - [http 헤더 구조](https://12bme.tistory.com/325)
