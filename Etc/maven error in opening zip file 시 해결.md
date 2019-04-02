## maven error in opening zip file 시 해결
인텔리제이 업뎃후 maven build 시 위와같은 에러를 뿜으면서 실행이되지않음  
인터넷에 찾아보니 jar 파일이 깨져서라는데.. 왜그런현상이발생한거냐!!!!!!!!  
보편적으로는 밑에 방법을 해결해서 사용했다고 하는데... 나는 전혀 해당이되지않음 🤬 😡😱

일단 사람들이 해결한 방법은 참고사이트에서 가져온 방법들
- local repository /.m2/repository 밑을 다날려줌!!
- 그다음 maven -U clean install 
- intellij invalidate Caches/Restart
- maven update 

위와같이해도 적용되지않았음
진심울뻔했다...........ㅠㅠㅠㅠㅠㅠㅠㅠ하 😭
일단 내가해결한 방법
- 발생한 깨진 maven repository에 경로로 들어가서 [MavenRepository](https://mvnrepository.com/)에서 해당 jar 파일을 다운받음
- 깨진 jar 파일이 있는 해당경로로(/.m2/repository/org/spring/블라블라!!)가서 다운받은 jar 파일을 덮어씌어줌 
- Open Module Setting 에 들어가서 Modules dependencies에서 Export 목록에 해당 파일이있는 경로를 클릭
- 하단 연필모양으로 클릭후 빨간색으로 되어있거나, 해당 jar파일을 해당경로로(/.m2/repository/org/spring/블라블라!!)에서 다시 넣어줌
- apply 후 Ok 해준다음 해당프로젝트 rebuild 해줌

진심 수동으로 다해줘서 해결함...  
나는 위에방법이 다안막혔었기때문에 /.m2/ 밑에 있어도 제대로 임포트가 되지않았음  
왜인지는모르겠으나 해결했으니 그걸로만족🏻



## 참고사이트
- [Maven : error in opening zip file when running maven](https://stackoverflow.com/questions/7600028/maven-error-in-opening-zip-file-when-running-maven)
- [maven build 에러 해결방법](https://zorba91.tistory.com/entry/maven-build-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95)
