## 원격 브랜치 삭제하기

~~~
git push 원격이름 --delete 브랜치이름
~~~


- origin 원격에 test1 이라는 브랜치를 지운다는 뜻
~~~
git push origin --delete test1
~~~

- 만약에 아래와 같은 오류가 발생하면 
~~~
error: unable to push to unqualified destination: the_remote_branch The destination refspec neither matches an existing ref on the remote nor begins with refs/, and we are unable to guess a prefix based on the source ref. error: failed to push some refs to 'git@repository_name'
~~~

- 밑에과 같은 명령어를 쳐준다
~~~
git fetch -p
~~~


## Git 리모트에서 삭제된 브랜치 업데이트하기
~~~
 git remote prune origin
~~~
- 해당 명렁어를 치면 로컬에 브랜치가 업데이트 되고 삭제된 브랜치가 보이지 않는다.


## 참고사이트
- [Git: Delete a branch (local or remote)](https://makandracards.com/makandra/621-git-delete-a-branch-local-or-remote)