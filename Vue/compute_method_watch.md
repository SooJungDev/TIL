## computed
- 복잡한 로직의 경우 반드시 계산된 속성을 사용

~~~html
<div>
    <p>원본메세지: {{message}}</p>
    <p>뒤집히도록 계산된 메세지:{{reverseMessage}}</p>
</div>
~~~
~~~javascript
var vm = new Vue({
    el:'#example',
    data:{
        message:'안녕하세요'
    },
    computed:{
        //계산된 getter
        reversedMessage: function(){
            //this는 vm 인스턴스를 가리킵니다.
            return this.message.split('').reverse.join('')
        }
    }
})
~~~

## methods와 computed 차이
- computed: data 속성에 변화가 있을때 자동으로 다시 연산 변화가 있을 경우에는 다시 연산하고 한번 연산한 값을 캐싱해놓았다가 다시사용함 , 같은 페이지내에서 같은 연산을 여러번 반복해야될 경우 성능면에서 효율적
- methods: 캐싱이라는 개념이 없기때문에 매번 재렌더링   
결론:캐싱효과가 필요하면 computed 사용 캐싱이 필요없다면 methods

## watch
- 데이터 변경에 대한 응답으로 비동기식 또는 시간이 많이 소요되는 동작을 수행하는 경우 유용

~~~javascript
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5
      }
    }
  },
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 문자열 메소드 이름
    b: 'someMethod',
    // 깊은 감시자
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    },
    // 콜백은 관찰이 시작된 직후 호출됩니다.
    d: {
      handler: function (val, oldVal) { /* ... */ },
      immediate: true
    },
    e: [
      function handle1 (val, oldVal) { /* ... */ },
      function handle2 (val, oldVal) { /* ... */ }
    ],
    // watch vm.e.f's value: {g: 5}
    'e.f': function (val, oldVal) { /* ... */ }
  }
})
vm.a = 2 // => new: 2, old: 1
~~~
- 화살표 함수를 감시자에 사용하면 안됨
  


##참고사이트

https://kr.vuejs.org/v2/guide/computed.html  
http://hong.adfeel.info/frontend/%EA%B3%84%EC%82%B0%EB%90%9C-%EC%86%8D%EC%84%B1computed-%EA%B0%90%EC%8B%9C%EC%9E%90watch/


