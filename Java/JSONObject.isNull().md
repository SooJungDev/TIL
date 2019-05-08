## JSONObject 특정 키 널 체크 값 하는법

- JSONObject.getJSONObject("key") 해당 객체에 없는데 호출하는경우 JSONObject is not a JSONObject 이런식으로 에러가 떨어짐
- 해당 키의 객체가 널인지 아닌지 체크하기위해서 isNull 메소드를 사용해서 널일경우 true 아닐경우 false

~~~java
//JSONObject json 객체가 있다고치면
json.isNull("해당키")
~~~

## 참고사이트
- [JSONObject api](https://stleary.github.io/JSON-java/org/json/JSONObject.html#getJSONObject-java.lang.String-)