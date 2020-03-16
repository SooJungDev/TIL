## 깃 온라인 코드리뷰 정리
- fork 한 저장소를 자신의 컴퓨터로 clone 한 후 디렉토리로 이동
~~~
git clone -b {본인_아이디} --single-branch https://github.com/SooJungDev/java-racingcar
~~~
~~~
// clone 한 폴더로 이동하는 방법 
cd {저장소 아이디}
ex) cd java-racingcar
~~~

- 기능구현을 위한 브랜치 생성
- 터미널에서 다음 명령을 입력해 브랜치를 생성한다
~~~
git checkout -b 브랜치이름
eX)git checkout -b step1
~~~

- 기능 구현 후 add,commit
~~~
git status // 변경된 파일 확인
git add -A (또는 .) // 변경된 전체 파일 한번에 반영
git commit -m "메세지" //작업한 내용을 메세지에 기록
~~~

본인 원격저장소에 올리기
~~~
git push origin 브랜치이름
ex) git push origin step1
~~~
