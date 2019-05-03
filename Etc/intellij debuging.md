## 인텔리제이 디버깅
- 한번 디버깅 한다음에 다시 실행 시키고 싶을 pause 눌렀다가 resume program 클릭 하면 다시 내가 건 위치에 디버그가 걸림

- Break Point 우클릭 하면 조건으로 break 걸수 있음
    - for,while 반복문일때 특정 값이 들어올때만 break 할때 편함

디버깅 버튼
- resume : 단축키 option+command+r 다음 break point 로 이동
- Step over: fn +f8 현재 break 된 파일에서 다음라인 이동
- step into: fn+f7 현재 break 된 라인에서 실행하고있는 라안으로 이동한다
- Force step into: option+shift+f7 다음 실행되는 라인으로 이동하나 step into 와 달리 stepping 을 무시하고 진행한다.
- step out: shift+f8 현재 break 된 라인에서 호출한 곳으로 이동한다.
- Drop frame: call stack 을 거슬러 올라간다
- Run to cursor: option+f9 포커스 되어있는 라인으로 이동
    - 단발 성으로 break 걸고 싶을때 사용 

Evaluate
- 계산기처럼 생긴 버튼 클릭 
- 여기서 확인하고 싶은 코드를 입력하고 실행시키면 결과를 바로 알 수 있음
    - 현재 라인에서 사용 가능한 코드
    
Watch
- Evaluate 와 동일한 기능 안경 모양같은거를 클릭해준다 

Call Stack
- 디버깅 화면 좌측 하단 threads 해당 break line 오기까지 call stack 이 출력된다


## 참고사이트
- 밑에 아주잘되어잇
- [IntelliJ 디버깅 해보기](https://jojoldu.tistory.com/149)