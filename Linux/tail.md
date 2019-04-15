## tail
- 문서 파일이나 지정된 데이터의 마지막 줄을 보여주는데 사용하는 유닉스 및 유닉스 계열 시스템에서의 프로그램

- tail 명령 구문은 다음과 같음
~~~
tail [옵션] <파일 이름>
~~~

- 파일 감시시 유용함
- tail은 실시간으로 파일 변화를 감시 할 수잇게 해주는 -f 옵션이 있
~~~
tail -f /var/adm/messages
~~~

- 파일 감시를 끝내고자한다면 ctrl+c 를 눌러서 빠져나옴
    - 정기적으로 감시하고싶으면 watch 사용한다.


## 참고사이트
- [tail wiki](https://ko.wikipedia.org/wiki/Tail)