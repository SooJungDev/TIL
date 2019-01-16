## 변수 속성으로 function 추가한 이유
~~~javascript
var init = function() {
  
}

var save = function() {
  
}
~~~
- 똑같은 이름을 가진 function이 중복되는 경우 
- 브라우저의 scope는 공용으로 쓰이기 때문에 나중에 불려진 js의 init, save function이  먼저 불러진 function을 덮어쓰게됨
= 이런 문제를 피하려고 변수, function영억으로 var main 이라는 객체 안에서 function을 선언
=> 이렇게되면 해당 파일 객체안에서만 이름이 유효하기 떄문에 다른 js와 겹칠 위험이 사라짐

## 참고사이트
  - [변수 속성으로 function 추가한 이유](https://jojoldu.tistory.com/255?category=635883)