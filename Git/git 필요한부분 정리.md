
## git 브랜치 전략
크게 3가지 있음. 
- git flow 
![2018-06-27 3 27 20](https://user-images.githubusercontent.com/38197944/41967314-4a0bc2d8-7a3c-11e8-8f96-0d7f6b5ab321.png)

기본 브랜치 5가지를 이야기하고 feature > develop > release > hotfix > master  브랜치가 존재. 
머지 순서는 앞에서 뒤로 진행됨. 
release ,hotfix 브랜치의 경우 develop 브랜치 오른쪽에 존재하기 때문에 모두 develop 브랜치도 머지하도록 구성

구조와 흐름. 
가장 중심이 되는 브랜치는 master,develop며 이 브랜치는 무조건 있어야함 
머지된 feature, release, hotfix 브랜치는 삭제하도록함

Feature 브랜치
- 브랜치 나오는곳 : develop
- 브랜치가 들어가는 곳 : develop
- 이름 지정: master,develop, release-*, hotifix-* 를 제외한 어떤 것이든 가능

새로운 기능을 추가하는 브랜치. 
feature 브랜치는 origin에 반영하지 않고, 개발자의 repository에 존재하도록 함.(머지할떄 --no--ff 옵션으로 머지가 되었음을 git기록에 남김)

Rease 브랜치
- 브랜치 나오는곳 : develop
- 브랜치가 들어가는 곳: develop, master
- 이름 지정: release-*

새로운 Production 릴리즈를 위한 브랜치
지금까지 한 기능을 묶어 디벨롭 브랜치에서 릴리즈 브랜치를 따냄. 
디벨롭 브랜치에서는 다음 릴리즈에서 사용할 기능을 추가한다.   
release브랜치에서는 버그픽스에 대한 부분만 커밋하고, 릴리즈 준비가 되었으면 master로 머지함

master로 머지후 tag명령을 이용하여 릴리즈 버전에 대해 명시하고 옵션을 이용하여 머지한 사람의 정보도 남겨두도록 함. 
그런뒤 develop브랜치로 머지하여 release브랜치에서 수정된 내용이 develop브랜치에 반영되도록 함

Hotfix 브랜치
- 브랜치 나오는곳 :master
- 브랜치 들어가는곳: develop, master
- 이름 지정: hotfix-*

Production에서 발생한 버그들은 전부 여기로 , 수정 끝나면 develop, master브랜치에 반영
master에는 tag를 추가함.
만약 release 브랜치가 존재한다면, release브랜치에 hotfix브랜치를 머지하여 릴리즈될때 반영이 될 수 있도록함

장점: 명령어가 나와있음, 웬만한 에디터와 IDE에는 플러그인으로 존재
단점:브랜치가 복잡, 안쓰는 브랜치가 있으며, 몇몇브랜치는 애매한 포지션

- GitHub flow
![2018-06-27 5 38 36](https://user-images.githubusercontent.com/38197944/41967343-5ece88ea-7a3c-11e8-804d-aaedd3706705.png)

git flow가 좋은방식이긴 하지만 github에서 사용하기 복잡하다 여겨 사용하지 않고 github flow라는 내용으로 사용. 
master 브랜치에 대한 role만 정확하다면 나머지 브랜치들에는 관여하지않음. 
그리고 pull request 기능을 사용하도록 권장. 

특징. 
release 브랜치가 명확하지 않는 시스템에서 사용에 맞게 되어있음
hotfix와 가장 작은 기능을 구분하지않음 

어떻게 사용 할 것인가???
1.master 브랜치는 어떰 때든 배포가 가능하다
2.새로운 일을 시작하기 위해 브랜치를 master에서 딴다면 이름은 어떤일을 하는지 명확하게 작성한다
3.원격지 브랜치로 수시로 push를 한다.
4.피드백이나 도움이 필요할떄, 그리고 머징 준비가 완료되었을때는 pull request를 생성한다.
5.기능에 대한 리뷰와 사인이 끝난후 master로 머지한다.
6.master로 머지되고 푸시 되었을때는 즉시 배포되어야 한다

장점:브랜치전략이 단순, github 사이트에서 제공하는 기능 모두 사용하여 작업 진행,코드리뷰 자연스럽게 ,CI가 필수적이며, 배포는 자동으로 진행. 
단점: CI와 배포 자동화가 되어있지 않은 시스템에서는 사람이 관련된 업무를 진행

- GitLab Flow 
github에서 말하는 flow는 너무간단하여 배포,환경구성,릴리즈,통합에 대한 이슈들이 많음 그것을보안하기 위해 gitlab에서 관련 내용들을 추가적으로 덧붙임

Production branch with GitLab flow
<img width="562" alt="2018-06-27 7 01 26" src="https://user-images.githubusercontent.com/38197944/41967508-b97dc5da-7a3c-11e8-9ce9-f25fb58b3522.png">

production브랜치가 존재하여 커밋한 내용들을 일방적으로 디플로이 하는 형태
github에서 브랜치 하나를 더 구성하여 사용함 

Environment branches with GitLab flow
<img width="619" alt="2018-06-27 7 01 42" src="https://user-images.githubusercontent.com/38197944/41967552-d0e8bd24-7a3c-11e8-88a3-358079eccc8e.png">
master와 production사이에 pre-production을 두어 개발한 내용을 곧장 반영하지 않고 시간을 두고 반영하는 것을 말함 

Release branches with GitLab flow
<img width="634" alt="2018-06-27 7 01 52" src="https://user-images.githubusercontent.com/38197944/41967608-ebea96b0-7a3c-11e8-8ebe-867d432dfc70.png">
release한 브랜치를 두고서 보안상 문제가 발생하거나 백포트를 위해 작업할 경우 cherry-pick을 이용해서 작업을 진행 할 수도 있음
아니면 해당 릴리즈에서 발생하는 버그들을 묶어서 수정하는 방식을 진행하면됨 
일반적으로 말하는 upstream first정책이라고 보면됨



#fork 한 프로젝트를 최신상태로 유지하기
- 소스트리 기준
1.원격(remote)를 추가해준다
remote 옆 new remote 클릭 또는 새원격 클릭

2. 원격이름을 upstream으로해주고 소스트리 github 계정을 연결해 놓았을 경우 지구본 모양을 클릭하면 
   원본 git url 클릭하거나 연결되지 않았을때 원본 git 주소를 입력해줌

3.upstream 옆에 pull from upstream 또는 upstream에서 가져와 병합하기 클릭

4.팝업창이 열리고 가지고 오고 싶은 브랜치를 선택한 다음에 확인. 
팝업창 옆에 가져오기 위한 원격 브랜치 아무것도 없다면 그옆에 새로고침 클릭하면 목록이뜸 

5.최신상태로 업데이트되고 업데이트 된것을 자신의 저장소로 push

## 참고사이트
[pull request 하는법](https://medium.com/axisj/github-fork-%EC%97%90%EC%84%9C-pull-request-%EA%B9%8C%EC%A7%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-merge-a22bdd097283)
[브랜치전략 자세히나옴](https://ujuc.github.io/2015/12/16/git-flow-github-flow-gitlab-flow/)
