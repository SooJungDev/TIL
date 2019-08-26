## nginx 프록시 설정
- 기본 설치시 세팅 파일 위치
~~~
/etc/nginx/nginx/nginx.conf 
~~~

- 하지만 아래 경로에서 변경해야함 nginx.conf 는 default.conf를 바라봄
~~~
/etc/nginx/conf.d/default.conf
~~~


- 서버마다 세팅되어있는데 아래와 같이 세팅해줄수있음
- AA 번으로 들어온 요청을 BBBB 포트로 다시 reverse proxy 형태로 넘겨주는 방식에 대한 코드
~~~
server {

    listen AA;

    access_log /var/log/프록시서버log폴더명/access.log;

    error_log /var/log/프록시서버log폴더명/error.log;



    location / {

        proxy_pass_header Server;

        proxy_set_header Host $http_host;

        proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Scheme $scheme;

        proxy_pass http://전달할 주소:BBBB;

    }

}

~~~

- 위에서 세팅 해주므로 위에 설정을바꿔줌 설정 변경후 재시작해줘야됨
~~~
sudo nginx
~~~


## 참고사이트
- [Proxy 설정 및 기본세팅](https://annajinee.tistory.com/15)
- [nginx를 이용한 Reverse Proxy 서버 구축](https://interconnection.tistory.com/27)