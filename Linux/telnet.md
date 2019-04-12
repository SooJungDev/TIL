telnet
- 목적지 서버의 해당 어플리케이션까지 살아 있는지를 확인함

telnet 사용법
~~~
telnet [목적지 ip주소] [어플리케이션 port 정보]
ex) telnet 204.111.111.1 9002
~~~

사용결과 정상이면
~~~
telnet 204.111.111.1 9002

trying 204.111.111.1

Connected to 204.111.111.1

Escape character is '^]'.

~~~

아니면 
~~~
telnet: Unable to connect to remote host: Connection refused

~~~


## 참고사이
- [ping, traceroute, telnet 사용법](https://jink1982.tistory.com/131)