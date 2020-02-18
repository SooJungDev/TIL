## 멀티아이피 목록 확인하는 방법
- 아래의 명령어를 치면 해당 서버에 멀티아이피를 확인할수있음 
- 나올떄는 ctrl+c 로 나온다.
~~~
ifconfig -a | res
~~~
## 리눅스 프로세스 관계 확인
- pstree 명령어는 실행되고 있는 프로세스의 상관관계를 트리 형태로 출력 해주는 명령어로써 
관계를 트리 형태로 출력해주므로 계층관계를 한 눈에 파악 할 수 있다.
~~~
pstree
~~~
옵션들
- -a : 명령행에서 지정한 인수가 있다면 명령어 라인 인수까지 보여준다
- -c : 기본값은 동일한 트리 내의 같은 프로세스를 하나의 프로세스만 보여주고 해당 프로세시의 개수를 나타내는데, 같은 프로세스를
      모두 보여준다
- -G : 트리형태를 보기 좋게 VT100 형태로 보여준다
- -h : 현재 프로세스와 부모 프로세스를 하이라이트로 보여준다
- -H pid : pid로 지정된 프로세스와 부모 프로세스를 하이라이트로 보여준다
- -l : 긴라인을 모두 보여준다

## netstat  네트워크 상태 확인 방법
- 네트워크 접속, 라우팅 테이블, 네트워크 인터페이스의 통계 정보를 보여주는 명령
~~~
netstat -nap 
netstat -an | grep 포트번호 
netstat -nlpt
netstat -nap | grep LISTEN
~~~
- netstat -nap : 연결을 기다리는 목록과 프로그램을 보여준다
- netstat -an | grep 포트번호 : 특정 포트가 사용 중에 있는지 확인 
- netstat -nlpt : TCP listening 상태의 포트와 프로그램을 보여준다
- netstat -nap | grep LISTEN : 연결을 기다리는 목록과 프로그램을 보여주는데 LISTEN 상태만 보여준다.

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

tail 명령어등으로 지속적으로 출력되는 화면내 단축키

Ctrl + S: 화면 출력 일시 중지
Ctrl + Q: 화면 출력 중지 해제
Ctrl + Z: 현재 실행중인 명령을 잠시 멈춤
Ctrl + C: 현재 실행중인 명령을 종료. (kill의 기능)
Ctrl + D: 빠져나가기 명령으로써 로그아웃하는 명령어. (logout 또는 exit 명령어와 동일)


## Logstash 에러로그 확인 및 재시작
- 경로 위치
~~~
/var/log/logstash-forwarder
~~~
- 에러로그 확인
~~~
tail -1000f logstash-forwarder.err 
~~~
- 종료 및 재시작
~~~
service logstash-forwarder stop
service logstash-forwarder start
~~~

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
## 서비스 목록 확인하기
- --status-all: 모든 서비스의 상태를 출력한다. 이 옵션은 모든 서비스를 대상으로 하므로 서비스명을 주지 않아도 된다.
~~~
service --status-all
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

## 노후화 된 장리 교체로 인한 서버 재시작 시 팁
1. 해당 서버에 어떤 서비스들이 떠있는지 확인한다
확인 할 때는 아래와 같은 명령어로 확인
~~~
ps -ef | grep java

pstree

service --status-all

netstat -nap | grep LISTEN
~~~
2.실행할 서버가 많다면 실행 할 명령어들을 모아 실행파일로 만든다
- 종료를 한꺼번에 할 파일
- 시작을 한꺼번에 할 파일

~~~
vi shutdown-all.sh 
vi start-all.sh 
chmod +x shutdown-all.sh 
chmod +x start-all.sh 
~~~

- shutdown-all.sh 
~~~
sudo service nginx stop
톰캣경로1/bin/shutdown.sh
톰캣경로1/bin/shutdown.sh
톰캣경로1/bin/shutdown.sh
sudo service 서비스명 stop
sudo service 서비스명 stop
~~~

- start-all.sh 
~~~
톰캣경로1/bin/startup.sh
톰캣경로1/bin/startup.sh
톰캣경로1/bin/startup.sh
sudo service 서비스명 start
sudo service 서비스명 start
~~~

3.잘 실행 되었는지 확인
~~~
ps -ef | grep java
~~~

4.톰캣이나 서비스가 뜨는 시간이 있으므로 nginx start는 수동으로 진행함 
~~~
sudo service nginx start
~~~

5. nginx 잘 실행 됬는지 확인
~~~
service nginx status 
~~~

6.nginx 로그가 있는 경로로 가서 파일보고 확인
~~~
cd /var/log/nginx
ls -al
tail -f /var/log/nginx/access.log
~~~