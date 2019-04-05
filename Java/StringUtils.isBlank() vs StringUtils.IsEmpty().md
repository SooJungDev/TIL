## StringUtils 란
- org.apache.commos.langs.StringUtils 
- 자바의 String 클래스가 제공하는 문자열 관련 기능을 강화한 클래스
- 파라미터 값으로 null을 주더라도 절대로 NullPonintException을 발생시키지 않음

## StringUtils.isBlank() vs String.isEmpty() 차이점
- isBlank() 는 " " 띄어쓰기가 있는 공백도 잡아냄
- 반면에 isEmpty()는 "", 와 null 잡아냄


- isBlank()
~~~java
 StringUtils.isBlank(null)      = true
 StringUtils.isBlank("")        = true  
 StringUtils.isBlank(" ")       = true  
 StringUtils.isBlank("bob")     = false  
 StringUtils.isBlank("  bob  ") = false
~~~

- isEmpty()
~~~java
 StringUtils.isEmpty(null)      = true
 StringUtils.isEmpty("")        = true  
 StringUtils.isEmpty(" ")       = false  
 StringUtils.isEmpty("bob")     = false  
 StringUtils.isEmpty("  bob  ") = false
~~~



## 참고사이
- [StringUtils란](https://kmj1107.tistory.com/entry/Java-StringUtils)
- [StringUtils.isBlank() vs String.isEmpty() ](https://stackoverflow.com/questions/23419087/stringutils-isblank-vs-string-isempty)