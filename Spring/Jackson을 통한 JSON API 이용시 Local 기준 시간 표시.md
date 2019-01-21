## Jackson을 통한 JSON API 이용시 Local 기준 시간 표시
~~~java
 @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private Date updateDate;
~~~
- 위와 @JsonFormat 어노테이션을 사용해서 date를 내려준다

## 참고사이트
- [Jackson을 통한 JSON API 이용시 Local 기준 시간 표시](http://xenes.tistory.com/6)
- [JodaTime 과 jackson json library 연동](https://www.lesstif.com/pages/viewpage.action?pageId=24445204)