
## Controller 다운
https://github.com/naver/ngrinder/releases 사이트에서 다운받는다


## ngrinder-controller 실행 
ngrinder-controller 실행 
~~~
 java -XX:MaxPermSize=200m -jar ngrinder-controller-3.4.4.war -p 7777
~~~

 http://127.0.0.1:7777/

접속후 초기 id/비밀번호 
admin/admin

## agent 실행
agent 가 다운로드된 경로로 가서
ngrinder-agent 이동한뒤에
Agent 실행
~~~
./run_agent.sh
~~~

## 가중치 부여하여 테스트하기
~~~java
import net.grinder.scriptengine.groovy.junit.annotation.RunRate

@RunWith(GrinderRunner)
class TestRunner {
    ...
    @RunRate(50)
    @Test
    void doTest1() {
           ...
    }
  
    @RunRate(20)
    @Test
    void doTest2() {
           ...
    }
}


출처: https://junoyoon.tistory.com/entry/스크립트에-가중치를-부여하여-테스트-하기-Groovy-기법 [손으로 만드는 오픈소스]
~~~


참고사이트
- [nGrinder 이용하여 부하 테스트 해보기 ! (mac 기준 설치)](https://heedipro.tistory.com/279)
- [스크립트에 가중치를 부여하여 테스트 하기 - Groovy 기법](https://junoyoon.tistory.com/entry/%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%97%90-%EA%B0%80%EC%A4%91%EC%B9%98%EB%A5%BC-%EB%B6%80%EC%97%AC%ED%95%98%EC%97%AC-%ED%85%8C%EC%8A%A4%ED%8A%B8-%ED%95%98%EA%B8%B0-Groovy-%EA%B8%B0%EB%B2%95)