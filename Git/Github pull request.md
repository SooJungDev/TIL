## Github pull request
- 원본 Repository 에서 Fork 를 클릭해서 자신의 저장소로 옮겨온다.
- 작업 방식에따라서 다르겠지만 작업 내용을 자신의 저장소에 commit, push 한다.
- 작업내용이 있는 branch 우측을 클릭한다음 pull 요청 생성을 클릭한다.
- 클릭하면 pull request를 보내는 웹페이지로 이동하게 된다.
- 타이틀과 메세지를 작성한 후 create pull request 를 한다.


## 코드를 최신상태로 유지하는 방법 (소스트리기준)
- 1.원격(remote)을 추가해준다.
    - remote 옆에 new remote 클릭 또는 새원격 클릭
- 2.원격 이름을 upstream 으로 해주고 소스트리에 github 계정을 연결해 놨을경우 
지구본 모양 클릭하면 소유자가 나 자신말고 원본 Repository  git url을 클릭한다.
    - github 계정을 은경 하지 않았다면 원본 git 주소를 입력하면됨
- 3.원격 목록에 upstream 이 추가되고 upstream 마우스 오른쪽을 클릭한다.
- 4.upstream 옆에 작은 창이 뜬다. pull from upstream 또는 upstream에서 가져와 병합하기를 클릭한다.
- 5.팝업창이 열리고 가지고 오고 싶은 브랜치를 선택한 다음 확인해준다.
- 6.내 저장소에 push 를 눌러주면 원격 저장소에서 변경사항이 내 저장소에 반영된다.


## 참고사이트
- [Github으로 코드리뷰 하기](https://github.com/ohgyun/using-github-for-code-reviews)
- [github fork 에서 pull request 까지 그리고 merge](https://medium.com/axisj/github-fork-%EC%97%90%EC%84%9C-pull-request-%EA%B9%8C%EC%A7%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-merge-a22bdd097283)
- [토스랩 코드리뷰방식](https://tosslab.github.io/codereview/2015/12/18/%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9D%B4%EB%A0%87%EA%B2%8C-%ED%95%98%EA%B3%A0-%EC%9E%88%EB%8B%A4.html)
- [카카오스토리 코드리뷰](https://tech.kakao.com/2016/02/04/code-review/)
- [매끄로운 코드리뷰를 돕는 10가지 방법](http://www.bloter.net/archives/238819)
- [코드리뷰 기법](http://bcho.tistory.com/276)