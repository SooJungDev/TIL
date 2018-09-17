## git이 추적하지 않는 untracked files 한꺼번에 삭제하기


~~~shell
git clean -f // 파일만 지우려면
git clena -fd //파일과 폴더를 지우려면
git clean -fd --dry-run // 지워질 파일과 폴더를 확인할수 있음 
~~~
- git clean -f 사용하면 추적하지 않는 파일 모두 지울수있고 디렉토리까지 지우려면 -d를 주면됨
-  --dry-run 옵션을 사용하면 지워질 파일을 모두 확인할 수있음
  
  ## 참고사이트
  - https://blog.outsider.ne.kr/1164