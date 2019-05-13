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