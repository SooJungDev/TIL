## 멀티아이피 목록 확인하는 방법

- 아래의 명령어를 치면 해당 서버에 멀티아이피를 확인할수있음 
- 나올떄는 ctrl+c 로 나온다.
~~~
ifconfig -a | res
~~~

## grep 으로 로그확인
- A 100 해당 라인 앞으로 100줄 더
- B 100 해당 라인 뒤로 100줄 더
~~~
grep -A 100 -B "keyword" xxxx.txt
~~~

- 예시
~~~
 grep -A 100 -B .json xxxx.txt
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