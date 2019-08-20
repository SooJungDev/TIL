## 운영에서 톰캣 실행시  runMode 옵션주기
- 나같은 경우에는 /bin/catailina.sh 에 설정되어있었음  
- DrunMode 를 추가해준다. 아래처럼
~~~
JAVA_OPTS="-d64 -server -Xms2048m -Xmx8192m -XX:PermSize=128m -XX:MaxPermSize=256m -DrunMode=stg"
~~~

- catalina.sh 파일에는 자바 옵션 설정 및 톰캣 로그 경로등 각종 설정을 저장할수있음 하지만 잘못 변경시 실수되어 문제 발생
- 톰캣에는 추가 옵션을 설정하는 다른 방법이 더있음 setenv.sh 파일 생성하고 추가 커스텀 옵션 설정할수있음


## 참고사이트
- [setenv.sh 옵션 설정](https://lucaskim.tistory.com/37)