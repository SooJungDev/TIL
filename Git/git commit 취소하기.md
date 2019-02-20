## git commit 취소하기

-완료한 commit을 취소해야 할 때가 있음

// 일단 commit 목록확인
~~~
git log
~~~

방법은 크게 3가지
- commit을 취소하고 해당파일들은 stage 상태로 working directory 보존
~~~
git reset --soft HEAD^
~~~

- commit을 취소하고 해당 파일들은 unstaged 상태로 working directory 보존

~~~
git reset --mixed HEAD^
~~~

- commit을 취소하고 해당 파일들은 unstaged 상태로 working directory 삭제

~~~
git reset --hard HEAD^
~~~


소스트리에서는 오른쪽 마우스 클릭후
"브랜치 이름" 커밋으로 초기
<img width="743" alt="2019-02-20 6 09 10" src="https://user-images.githubusercontent.com/38197944/53079893-c621f400-353a-11e9-9c9b-2b0638e8ed0f.png">
