## log 파일의 종류
### catalina.out
  - 톰캣 기동 시 ${INSTANCE_DIR}/bin/catalina.sh 에 의해 생성되는 로그파일이다.
  
~~~
…
if [ -z "$CATALINA_OUT" ] ; then
  CATALINA_OUT="$CATALINA_BASE"/logs/catalina.out
fi
…
>> "$CATALINA_OUT" 2>&1 "&"
~~~ 
- 자바 기동시 standard output 과 standard error 를 모두 ${CATALINA_OUT} 으로 보냄 
- catalina.out 은 기본적으로 tomcat이 기동되어 있는 동안에는 단하나의 파일에 계속 내용이 추가된다. 따라서 catalina.out 의 크기가 계속 늘어날수있음
    - 보통은 로그포제이로 설정해준다음에 해당 catalina.out 을 적재하지 않는다!!!!!!!!

### catalina.yyyy-mm-dd.log
- ${INSTANCE_DIR}/conf/logging.properties 에 의하여 생성되고 제어
- catalina.YYYY-MM-DD.log 파일 **standard output, standard error** 와 같은 로그는 **제외**됨
    - 그 내용은 오직 **catalina.out**에서만 찾아볼수 있음

### 기타로그
- manager.yyyy-mm-dd
- host-manager.yyyy-mm-dd
- localhost_access_log.yyyy-mm-dd

=> 매니져 혹은 호스트매니져 사용시 로그와 톰캣 액세스 로그이다.

### Access 로그
- 사용자 request 기록을 위한 access 로그는 ${INSTANCE_DIR}/conf/server.xml 파일내에서 org.apache.catalina.valves.AccessLogValve를 활성화 함으로서 생성 가능
- 톰캣 버전에 따라 다름 기본적으로는 비활성화 되어있음


## 참고사이트
- [Apache Tomcat(톰캣) 로그에 대하여 (Java Logging과 JULI)](https://sarc.io/index.php/aws/900-apache-tomcat-java-logging-juli)