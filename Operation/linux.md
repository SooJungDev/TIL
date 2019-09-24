## 멀티아이피 목록 확인하는 방법

- 아래의 명령어를 치면 해당 서버에 멀티아이피를 확인할수있음 
- 나올떄는 ctrl+c 로 나온다.
~~~
ifconfig -a | res
~~~

## grep 으로 로그확인
~~~
tail -n1000 | grep -i "keyword" catalina.out
~~~
- tail -n1000 n만큼 라인을 출력 마지막부터 1000줄
- grep -i 대소문자 구별없이 찾는다.

~~~
grep -A100 -B100 "keyword" catalina.out
~~~
- 해당 키워드에 뒤로 100줄 앞으로 100줄 찾을수있음

## 톰캣
- 톰캣 프로세스 확인
~~~
ps -ef | grep tomcat
~~~

- 킬 명령어로 해당 pid 강제종료
~~~
kill -9 해당pid
~~~

- 톰캣이 있는곳에서 ~/bin 밑에서 실행시켜준다 shutdown.sh 
~~~
./shutdown.sh
~~~

- 톰캣이 있는곳에서 ~/bin 밑에서 실행시켜준다 startup.sh 
~~~
./startup.sh
~~~


## 실시간으로 로그 확인하기
- 로그파일 위치로가서 tail 에 -f 옵션을주면 확인할수 있음
- 빠져나가는거 ctrl+c
~~~
tail -f 로그파일이름
~~~


## Nginx 로그확인하기

- 에러로그 위치는 /var/log/nginx/error.log
~~~
tail -f /var/log/nginx/error.log
~~~

- -f 옵션 앞에 숫자를 주면 길게 볼수 있음 
~~~
tail -2000f catalina.out
~~~

- Nginx의 서버 접근 기록 실시간 모니터링
~~~
tail -f /var/log/nginx/access.log
~~~

- 시스템 에러 및 하드웨어 에러로그 위치 /var/log/messages
- /var/log/secure : 사용자 인증관련 로그

## Nginx ssl 설정파일 위치 및 확인방법

- 해당 디렉토리 밑으로 들어간다.
~~~
cd /etc/nginx/conf.d/
~~~

- 해당 파일 확인방법
~~~
cat 이름.conf | less
~~~

## nginx 멈추가

- 멈춘다.
~~~
sudo nginx -s stop
~~~

## nginx 시작

- 시작한다
~~~
sudo nginx 
~~~


## nginx 상태 확인하기
~~~
service nginx status
~~~


## 스프링부트 서비스 확인하고 서비스 올렸다가 내리기

- 서비스명으로 확인
~~~
ps -ef | grep 서비스명
~~~

- 해당 서비스 중지하기
~~~
sudo service 서비스명 stop
~~~

- 해당 서비스 시작하기
~~~
sudo service 서비스명 start
~~~

혹시 서비스명 모를때
- ex) 80포트 찾을떄
~~~
lsof -i :80
~~~

## Linux 종류 버젼 확인
~~~
grep . /etc/*-release
~~~

- 참고사이트 [리눅스 종류 확인, 리눅스 버전 확인](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EC%A2%85%EB%A5%98_%ED%99%95%EC%9D%B8,_%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%B2%84%EC%A0%84_%ED%99%95%EC%9D%B8)

## bind error address already in use 해결방법

- netstat 를 이용해 현재 사용중인 port 에대한 pid 를 검색한다.
 - 나는 해줬더니 pid 가 - 이런식으로 나와서 sudo 를 붙여줬더니 잘나왔다!
~~~
sudo netstat -lntp         
~~~

- 해당 호트를 사용하는 프로세스를 kill 해준다
~~~
sudo kill -9 pid
~~~

- 그러고 해당서비스를 다시실행시켜줬다.
~~~
sudo service 서비스명 start
~~~


## 용량 확인하기

- 현재 서버의 파일 시스템별 디스크 사용량 점검하는 명령어 df 
~~~
df -h
~~~

- 현재 위치하는 디렉토리의 파일사이즈를 보여줌
~~~
du -sh *
~~~

- 로그파일이있는 해당 위치로 가서 로그파일을 연도별로 정리
~~~
rm -rf *2017*
~~~

- 달별로 정리 
~~~
rm -rf *2019-01*
~~~

- 다시 정리되었는지확인 df -h 로 확인
~~~
df -h
~~~

-  catalina.out 초기화하고싶을때 
~~~
cat /dev/null > catalina.out
~~~


## 파일 끝으로 가고싶을때
- :$  하면 파일끝으로 가짐

## 명령어 작성중에 앞으로 커서를 옮기고싶을때
- ctrl+a

## 명령어 히스토리 보고싶을때
- history 

## 히스토리에 쳤던 명령어 그대로 입력하기 
- history쳐서 명령어 숫자 확인
- ! 명령어 숫자 입력

## 전에 쳤던 명령어 쉽게 찾는법
- ctrl + r 

## 크론 탭 스케쥴러 설정 여부 확인
- 관리자 상태로 전환
~~~
sudo su
~~~

- 작업 스케줄러 설정 여부확인
~~~
crontab -l 
~~~

## ip 확인
- ip 확인하기
~~~
ifconfig
~~~

- 외부로 나가는 ip 확인하기
~~~
curl bot.whatismyipaddress.com
~~~
## 리눅스 백그라운드 실행방법
- 리눅스나 유닉스의 터미널이 종료되도 살아있는 프로세스로 실행하는 방법
~~~
명렁어 &
~~~

- 터미널이 종료되도 계속해서 백그라운드로 실행되는 프로세스를 만들기 위해서는 nohup 명령을 사용
~~~
nohup 명령어 &
~~~

- 아래 실행을 & 백그라운드로 보낸다 
- 결과적으로 로그아웃/세션 타임 아웃이되어도 실행된 커맨드의 화면출력 결과를 /dev/null 로 버리고 에러메세지도 화면에 출력하지 않음
~~~
nohub command 1>/dev/null 2>&1&
~~~
- 참고사이트
 - [nohup 을 이용한 백그라운드 작업](https://ourcstory.tistory.com/197)


