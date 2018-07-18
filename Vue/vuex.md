
## Vuex란?
vue.js의 상태관리를 위한 패턴이자 라이브러리

## 상태관리(State Management)가 왜 필요한가
컴포넌트 기반 프레임워크에서는 화면 구성을 위해 화면 단위를 매우 잘개 쪼개서 컴포넌트로 사용한다.  
 작은단위들이 컴포넌트가 되어 한 화면에서 많은 컴포넌트를 사용하게됨.  
 이에따라 컴포넌트 간의 통신이나 데이터 전달을 좀 더 유기적으로 관히 할 필요성이 생김  

 <b>컴포넌트 간 데이터 전달 및 이벤트 총신등의 여러 컴포넌트의 관계를 한 곳에서 관리하기 쉽게 구조화 하는것이 State Management이다.</b>


 ## 상태관리로 해결 할 수 있는 문제점?
 상태관리는 중대형 규모의 앱 컴포넌트들을 더 효율적으로 관리하기 위한 기법임
 일반적으로 앱의 규모가 커지면서 생기는 문제점들 아래와같음

 1. Vue의 기본 컴포넌트 통신방식인 상위0하위에서 중간에 거쳐야 할 컴포넌트가 많아지거나
 2. 이를 피하기 위해 Event Bus를 활용하여 상하위 관계가 아닌 컴포넌트 간 통신시 관리가 되지 않는점   

이러한 문제점들을 해결하기 위해 모든 데이터 통신(state)을 한곳에서 중앙 집중식으로 관리한다.  
![2018-07-18 10 04 13](https://user-images.githubusercontent.com/38197944/42860540-094c9f2c-8a92-11e8-9e09-1c6d878673ca.png)




## 상태관리 패턴
상태관리 구성요소는 크게 3가지가 있다
- state:컴포넌트 간 공유될 data
- view: 데이터가 표현될 template
- actions: 사용자의 입력에 따라 반응할 method
 
 ~~~javascript
 new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
~~~
위 구성요소는 아래와 같이 동작한다 

![2018-07-18 10 55 22](https://user-images.githubusercontent.com/38197944/42860558-1de5be8c-8a92-11e8-82f2-19521a124686.png)

## 설치 및 등록
~~~ shell
npm install vuex --save
~~~

그리고 vuex를 등록할 js파일을 하나 새로 생성한다. 이름은 store.js로 지정
~~~javascript
// store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

export const store= new Vuex.Store({

});
~~~

그리고 Vue App이 등록된 main.js로 넘어가서 store.js를 불러와 등록하면된다.
~~~ javascript
//main.js
import Vue from 'vue'
import App from './App.vue'
//store.js를 불러와
import {store} from './store'

new Vue({
    el:'#app',
    //Vue 인스턴에 등록하다
    store,
    render: h => h(App)
})
~~~

## State 등록
state를 Vuex에 아래와 같이 추가할 수있다.
~~~javascript
//store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

export const store = new Vuex.Store({
    //counter라는 state 속성을 추가
    state:{
        counter:0
    },
});
~~~
<b>State는 컴포넌트간에 공유할 data속성을 의미한다.</b>

## state 접근
~~~html
<div id="app">
  Parent Counter : {{this.$store.state.counter}}<br>
  <button @click="addCounter">+</button>
  <button @click="subCounter">-</button>

  <!-- 기존코드 -->
  <!-- <child v-bind:passedCounter="counter"></child> -->
  <child></child>
</div>
~~~

~~~javascript
//App.vue
import child from './Child.vue'
export default{
    //기존코드
    //data (){
    // return {
    //    counter:0
    //  }
   // },
   methods:{
       addCounter(){
           this.$store.state.counter++;
       },
       subConter(){
           this.$store.state.counter--;
       }
   },
   components:{
       'child':Child
   }
}
~~~

기존코드와의 차이점은
1. data 속성으로 인한 선언 counter값 제거
2. child 컴포넌트로 counter를 전달하지않음

결국 Parent에서 관리하던 counter라는 데이터를 Vuex state로 넘겨준것이다.
따라서 Vuex라는 저장소의 데이터를 모든 컴포넌트들이 동일한 조건에서 접근하여 사용하게된다.
vuex state를 이용하여 데이터 관리를 한곳에서 효율적으로 할수있음 


## Getters란?
중앙 데이터 관리식 구조에서 발생하는 문제점중 하나는 각 컴포넌트에서 Vuex의 데이터를 접근할때 중복된 코드를 반복호출하게 되는 것이다.

예를 들어 아래와 같은 코드가 있다

~~~javascript

//App.vue 

computed:{
    doubleCounter(){
        return this.$store.state.counter * 2;
    }
}

//child.vue
computed{
    doubleCounter(){
        return this.$store.state.Counter * 2;
    }
}
~~~

여러 컴포넌트에서 같은 로직을 비효율적으로 중복 사용하고 있음.
이때 vuex의 데이터(state)변경을 각 컴포넌트에서 수행하는게아니라 Vuex에서 수행하도록 하고 각 컴포넌트에서 수행 로직을 호출하면, 코드 가독성 올라가고 성능에서도 이점이생긴다.

~~~javascript

//store.js(Vuex)
getters:{
    doubleCounter: function(state){
        return state.counter * 2;
    }
},

//App.vue
computed:{
    doubleCounter(){
        return this.$store.getters.doubleCounter;
    }
}

//child.vue
computed:{
    doubleCounter(){
        return this.$store.getters.doubleCounter
    }
}
~~~

## mapGetters
vuex에 내장된 helper함수 , mapGetter로 더 직관적이게 작성할수있음

~~~html
<!--App.vue -->
<div id="app">
    parent counter : {{parentCounter}}
</div>    
~~~

~~~javascript
//App.vue
import {mapGetters} from 'vuex'

//...
computed:mapGetters({
    parentCounter : 'getCounter' //getCounter는 Vuex의 getters에 선언된 속성 이름 
}),
~~~

주의할 점은 컴포넌트 자체에서 사용할 computed속성과 함께 사용 할수 없다는 점이다.
해결방안은 ES6의 문법 ...을 사용하면된다.

~~~javascript
import {mapGetters} from 'vuex'


computed:
...mapGetters([
    'getCounter'
]),
anotherCounter(){

}
~~~

## Mutations란?
Mutations이란 vuex의 데이터 즉 state값을 변경하는 로직들을 의미한다.(쉽게 말하면 setter)
Getters와 차이점은
1. 인자를 받아 vuex에 넘겨 줄 수 있고
2. computed가 아닌 methods에 등록
또한 Actions오아ㅢ 차이점이다
- mutations는 동기적 로직을 정의
- Actions는 비동기적 로직을 정의

<b>Mustations의 성격상 안에 정의한 로직들이 순차적으로 일어나야 각 컴포넌트의 반영 여부를 제대로 추적 할 수 있기 때문이다.</b>

##Mutations 등록
getter와 마찬가지로 Vuex에 mutations속성을 추가한다

~~~javascript
//store.js
export const store = new Vuex.Store({
    // ...
    mutations:{
        addCounter: function(state, payload){
            return state.counter++;
        }
    }
})
~~~

## Mutations 사용 
~~~javascript
//App.vue
    methods:{
        addCounter(){
            //this.$store.state.counter++;
            this.$store.commit('addCounter');
        }
    }
~~~

여기서 주목할 만한 부분은 getters처럼
this.$store.mustations.addCounter; 이런식의 접근이 불가능 하고 commit을 이용하여 mutations 이벤트를 호출해야 한다는 점이다.

## Mutations에 인자 넘기기
각 컴포넌트에서 vuex의 state를 조작하는 필요한 특정 값들을 넘기고 싶을때는 commit()에 두번째 인자를 추가한다.

~~~javascript
this.$store.commint('addCounter',10);
this.$store.commint('addCounter',{
    value: 10,
    arr:["a","b","c"]
});

//이를 vuex에서는 아래와 같이 받을수 있다
mutations:{
    //payload 가 {value:10}일 경우 
    addCounter: function (state, payload){
        state.counter= payload.value;
    }
}

~~~

<b>데이터 인자명은 보통 payload 많이쓴다.</b>


## mapMutations
mapGetters와 마찬가지로, vuex에 내장된 mapMutations를 이용하여 코드 가독성을 높일수 있다.

~~~javascript
//App.vue
import {mapMutations} from 'vuex'

methods:{
    //vuex의 Mutations메서드 명과 App.vue 메서드 명이 동일할때 [] 사용
    ...mapMutations([
        'addCounter'
    ]),
    //vuex의 mutations 메서드명과 app.vue 메서드명을 다르게 매칭할때 {} 사용
    ...mapMutations({
        addCounter:'addCounter' //앞 addCounter는 해당컴포넌트의 메서드를, 
        //뒤의 addcounter는 vuex의 Mutations를 의미 
    })
}
~~~

## Action란?
Mutations에는 순차적인 로직들만 선언하고 Actions에는 비순차적인 비동기 처리 로직들을 선언한다. 왜 나눠서 등록해야되는가?
Mutations의 역할 자체가 State 관리에 주안점을 두고있음. 상태 관리 자체가 한데이터에 대해 여러개의 컴포넌트가 관여하는것을 효율적으로 관리하기위함인데 Mutations에 비동기 처리 로직들이 포함되면 같은 값에 대해 여러개의 컴포넌트에서 변경을 요청했을때 그 변경 순서 파악이 어렵기 때문이다. 

<b>이러한 문제를 방지하기 위해 비동기 처리 로직은 Actions에 동기처리 로직은 Mutations에 나눠서 구현한다.</b>

따라서 setTimeout()이나 서버와의 http통신 처리 같이 결과를 받아올 타이밍이 예측되지 않는 로직은 Actions에 선언한다.

## Actions 등록
vuex에 Actions를 등록하는 방법은 다른속성과 유사하다. actions를 선언하고 action method를 추가해준다.

~~~javascript
//store.js
export const store = new Vuex.store({
    mustations:{
        addCounter: function(state, payload){
            return state.counter++;
        }
    },
    actions{
        addCounter:function(context){
            // commit의 대상인 addCounter는 mutations의 메서드를 의미한다.
            return context.commint('addCounter');
        }
    }
});
~~~

상태가 변화하는걸 추적하기 위해 actions는 결국 Mutations의 메서드를 호출(commit)하는 구조가 된다.

~~~ javascript
//store.js
export const store= new Vuex.Store({
    actions:{
        getServerData: function(context){
            return axios.get("sample.json").then(function(){

            });
        },
        delayFewMinutes: function(context){
            return setTimeout(function(){
                commit('addCounter');
            },1000);
        }
    }
})
~~~
위에처럼 HTTP get요청이나 setTimeout과 같은 비동기 처리 로직들을 actions에 선언해준다.

## Actions 사용
actions를 호출할때는 아래와 같이 dispatch()를 이용한다.
~~~javascript
//App.vue
methods:{
    //Mutations를 이용할때
    addCounter(){
        this.$store.commit('addCounter');
    }
    //Actions 를 이용할때
    addCounter(){
        this.$store.dispatch('addCounter');
    }
}
~~~

## Actions에 인자값 넘기기
Actions에 인자를 넘기는 방법은 Mutations와 유사하다.

~~~javascript
<!-- by와 duration등의 여러 인자 값을 넘길 경우 key,value 형태로 여러값을 넘길수있다 -->
<button @click="asynIncrement({by: 50, duration: 500})"></button>

~~~

~~~javascript
export const store = new Vuex.Store({
    actions:{
        //payload는 일반적으로 사용하는 인자명
        asyncIncrement: function(context. payload){
            return setTimeout(function(){
                context.commit('increment',payload.by);
            }, payload.duration);
        }
    }
})
~~~

## mapAction
mapGetters, mapMutations 헬퍼 함수들과 마찬가지로 동일하게 사용가능


## 참고사이트
진짜 글이 잘되있다..ㅠㅠ 감탄  
 - [vuex 시작하기3](https://joshua1988.github.io/web-development/vuejs/vuex-actions-modules/)
- [vuex 시작하기2](https://joshua1988.github.io/web-development/vuejs/vuex-getters-mutations/)
- [vuex 시작하기1](https://joshua1988.github.io/web-development/vuejs/vuex-start/)
