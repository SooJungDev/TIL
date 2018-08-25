## grep 
grep -nr “if  (std::find(.*begint” *
grep 커맨드란 global regular expression print 의 첫글자에서 따온말
파일의 표준 입력을 검색하여 정규표현식과 맞는 부분을 찾아주는 커멘드
파일내의 패턴을 검색하기위해 grep커맨드를 이용하는것은 아주좋은 방법

* -i : 대소문자 구별 안하기
* -r : recursive 옵션으로 폴더 내의 모든 파일에게 똑같은 커맨드 반복
* -l : 매칭라인과 파일 이름만 출력
* -v : 매칭되지 않는 라인만 출력
* -c : 매칭라인의 개수만 출력
* -n : 매칭된 라인 넘버 출력

## 커밋메세지 작성시
~~~
Use base::ContainsValue() instead of std::find() in ui/ozone

This patch simplifies conditions around the code in ui/ozone

Bug: 561800
Change-Id: I910244a9a50ff89b2979e9942e619752bbc319b1
~~~
메세지 작성후 꼭 해당 Bug를 써줘야됨

## 빌드
- ninja -C out/Debug chrome(chrome은 타겟임 해당 부분에만 있는 파일을 빌드함)  
- ninja -C out/Debug (할경우 전체를 하기때문에 시간이좀걸림 )  
- e.g) ninja -C build -j 20 changes into the build directory and runs 20 build commands in parallel.   
-j 옵션을 주면 병렬로처리해서 빨리 할수있음!!!!!!

## git
- git status (반영할 파일 상태 확인)
- git commit -a —amend
  git commit 명령을 실행할 때 -a 옵션을 추가하면 Git은 Tracked 상태의 파일을 자동으로 Staging Area에 넣는다.
  커밋 메시지를 수정하는 방법 git commit --amend  사용하면됨 
- git log(현재 commit 내역을 본다 )
- git cl upload(코드 업로드)
- git cl format (코드 포맷팅)

## 순서
1. grep으로 수정할 파일을 위치를 찾고
2. 해당 파일의 위치에서 작업하고 저장
3. git status로 변경된 파일을확인한후에
4. ninja -C out/Debug 로 빌드를 꼭해준다! 
5. git commit -a —amend 커밋메세지를 수정해주고
6. git log로 확인
7. git cl format 으로 코드를 포맷팅해주고 
8. git cl upload로 다시 업로드해준다.

## 참고사이트
- [grep](https://macinjune.com/all-posts/mac/terminal/%EB%A7%A5-%ED%84%B0%EB%AF%B8%EB%84%90unix-grep-command%EB%A1%9C-%ED%8C%8C%EC%9D%BC-%EB%82%B4%EB%B6%80%EC%9D%98-%ED%8C%A8%ED%84%B4-%EC%B0%BE%EA%B8%B0/)
- [ninja manual](https://ninja-build.org/manual.html)
- [git commit](https://backlog.com/git-tutorial/kr/reference/log.html)
- [git commit option -a](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%88%98%EC%A0%95%ED%95%98%EA%B3%A0-%EC%A0%80%EC%9E%A5%EC%86%8C%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0)

