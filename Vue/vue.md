# Vue는 무엇인가?

- MVVM 패턴 뷰모델(ViewModel)레이어에 해당하는 화면 (View)단 라이브러리
- 데이터 바인딩과 하면단위를 컴포넌트 형태로 제공, 관련 API를 지원하는데 궁극적인 목적이 있음
- Angular 에서 지원하는 2way data bindings을 동일하게 제공
- 하지만 Component 간 토인의 기본 골격은 React 의 1 Way DataFlow(부모 -> 자식)와 유사 
- 다른 Front-End와 비교했을때 상대적으로 가볍고빠름

## MVVM 패턴
MVC 방식에서 기안, 간단하게 생각해서 화면 앞단의 화면동작 관련 로직과 뒷단 DB데이터 처리 및 서버 로직을 분리하고, 뒷단에서 넘어온 데이터를 Model에 담아 view로 넘겨주는 중간 지점  
view  <->  ViewModel  -> Model

# Vue Instance
- 인스턴스는 Vue.js로 개발할때 필요한 근간 & 토대 (foundation)
- vue.js 라이브러리를 로딩하고나서 접근 가능한 Vue생성자로 인스턴스 생성
- Vue 객체를 생성할 때 아래와 같이 data,template, el,methods, life cycle callback 등의 인스턴스 옵션을 포함할 수 있다.
  
~~~javascript
//vm은  ViewModel을 뜻한다.
var vm =new Vue({
    //인스턴스 옵션 속성 
    template:...,
    el:...,
    methods:{

    },
    created:{

    }
})
~~~

위를 풀어서 애기하면 vue라는 변수에 화면에서 사용할 옵션(데이터 속성,메서드 등등) 포함하여 화면의 단위를 생성한다는 의미

## Vue instance 라이프싸이클 초기화 
Vue 인스턴스가 생성 될때 Vue({ })아래의 초기화 작업을 수행한다.
- 데이터 관찰: 데이터의 변경을 감지하는 반응성(Vue Reactivity)주입
- 템플릿 컴파일: 가상돔(Virtual DOM) -> 실제 돔 (DOM)으로 변환
- DOM에 인스턴스 부착: 템플릿의 내용을 실제 DOM에 연결
- 데이터 변경시 DOM 업데이트 

초기화 작업외에도 커스텀 로직을 아래와 같이 추가
~~~ javascript
var vm =new Vue({
    data:{
        a:1
    },
    created(){
        console.log('a is:'+this.a)
    }
})
~~~
인스턴스 생성 new Vue()
- beforeCreate: 이벤트 및 라이프 사이클 초기화
- created: 화면에 반응성 주입
- beforeMount: el, template 속성확인, template 속성 내용을 render()로 변환
- Mouted: vm.$el 생성후 el 속성 값을 대입
인스턴스를 화면에 부착
- beforeUpdate: 인스턴스 데이터 변경
- updated: 화면 재 렌더링 및 데이터 갱신
인스턴스 내용 갱신
- beforeDestory: 인스턴스 접근 가능
- destroyed: 컴포넌트 , 인스턴스, 디렉티브 등 모두해제
인스턴스 소멸 

# Vue components
- 화면에 비춰지는 뷰의 단위를 쪼개어 재활용이 가능한 형태로 관리하는 것이 컴포넌트
- Vue 인스턴스를 생성하기전에 꼭 Component 부터 등록
- 컴포넌트의 data속성은 꼭 함수로 작성해야함

~~~ html
<div id="app">
  <my-component></my-component>
</div>  
~~~
~~~javascript
//등록
Vue.component('my-component'.{
    template:'<div>A custom component!</div>',
    data:function(){
        return{
            text:'hello'
        }
        /*모든 컴포넌트가 같은 값을 공유하지 않게 하기위해서 함수로 작성*/
    }
})
//Vue 인스턴스 생성
new Vue({
    el:'#app'
})
~~~

## Global component
- 컴포넌트를 뷰 인스턴스 등록해서 사용할 때 다음과 같이 전역(global)으로 등록 할 수 있다.
~~~ javascript
Vue.component('my-component',{
    //...
})
~~~

## Local component
~~~javascript
var cmp={
    data:function(){
        return{
            //...
        }
    }
    template:'<hr>',
    methods:{}
}

//아래 Vue 인스턴스에서만 활용 할 수 있는 로컬(지역)컴포넌트 등록
new Vue({
    components:{
        'my-cmp':cmp
    }
})

~~~

## 부모와 자식 컴포넌트 관계
- 구조상 상-하 관계에 있는 컴포넌트 통신
   - 부모 -> 자식: props down
   - 자식 -> 부모: event up
  
  Props
  - 모든 컴포넌트는 각 컴포넌트 고유의 유효범위를 갖음
  - 상위에서 하위로 값을 전달하려면 props 속성을 사용한다.
  
  ~~~javascript
  Vue.component('child-component'.{
      props:['propsdata'],
      template:'<p><{{ propsdata }}</p>'
  });
  
  var app =new Vue({
      el:'#app',
      data:{
          message:'Hello Vue! from Parent Component'.
      }
  });
  ~~~

  ~~~ html
  <div id="app">
    <child-component v:bind:propsdata="message"></child-component>
  </div>
  ~~~
  js에서 props 변수 명명을 카멜기법으로 하면 html에서 접근은 케밥 기법(-)으로 가야한다.

  ## 같은 레벨의 컴포넌트 간 통신
  컴포넌트 간의 직접적인 통신은 불가능하도록 되어있는게 Vue.js 구조

  ## Event Bus - 컴포넌트 간의 통신
  Non Parent- child 컴포넌트 간의 통신을 위해 Event Bus를 활용 할 수 있음
   - Event Bus를 위해 새로운 Vue를 생성하여 Vue Root Instance 가 위치한 파일에 등록
  ~~~Javascript
  // Vue root Instance 전에 꼭 등록
  var eventBus = new Vue();

  new Vue({

  })
  ~~~
  
  ## 이벤트 발생, 수신
  - $emit('이벤트 명',인자) 으로 이벤트 발생
  - 이벤트를 발생시킬 컴포넌트에 eventBus import후 $emit으로 이벤트 발생
  ~~~javascript
  import { eventBus } from './main';
  eventBus.$emit('refresh',10);
  ~~~

  - 해당 이벤트를 받을 컴포넌트에서 $on('이벤트 명',인자)로 이벤트 수신 
  - 해당 이벤트를 받을때도 컴포넌트에서 동일하게 import 후 콜백으로 이벤트 수신 
  ~~~javascript
  import { eventBus } from './main';

  // 등록하는 위치는 해당 컴포넌트의 created 메서드에 등록
   created(){
       eventBus.$on('refresh',function(data){
           console.log(data);
       });
   }
  ~~~
  - eventBus 콜백함수 안에서 해당 소스의 메서드를 참고하려면 self 사용
  ~~~ javascript
    methods:{
        callAnyMethod(){
            //...
        }
    }
    created(){
        var self= this;
        eventBus.$on('refresh',function(data){
            console.log(this)// this는 빈 Vue 인스턴스를 접근
            self.callAnyMethod()//self는 이 created의 vue 컴포넌트에 접근 
        });
    }
  ~~~
 
 # Vue Routers
 - vue.js를 이용하여 SPA(Single Page Application)을 제작할때 유용한 라우팅 라이브러리
 - 뷰 코어 라이브러리 외에 router 라이브러리를 공식 지원, cdn, npm으로 설치
  ~~~ shell
  // npm 설치방법
  npm install vue-router --save
  ~~~
  - Vue 라우터는 기본적으로 RootUrl'/#/'{Router name}의 구조로 되어있다
   example.com/#/user
  - 여기서 URL의 # 태그를 제거하려면 history mode 사용
  ~~~javascript
    const router =new VueRouter({
        routes,
        //아래와 같이 history모드를 추가해주면 된다.
        mode:'history'
    })
  ~~~

  ## Nested Routers 
  - 라우터로 화면 이동시 Nested Router를 이용하여 여러개의 컴포넌트를 동시에 렌더링 할수 있음
  - 렌더링 되는 컴포넌트 구조는 가장 큰 상위 컴포넌트가 하위 컴포넌트를 포함하는 parent-child 형태
  
  ~~~ html
  <!--localhost:5000-->
  <div id="app">
    <router-view></router-view>
  </div>


  <!--localhost:5000/home -->
  <!--parent component -->
  <div>
    <p>Main Component renderd</p>
    <!--child component -->
    <app-header></app-header>
  </div>
  ~~~

  ~~~javascript
  // localhost:5000/home 에 접근하면 Main과 Header 컴포넌트 둘다 렌더링됨
  {
      path:'/home',
      component:Main,
      children:[
          {
              path:'/',
              component:AppHeader
          },
          {
              path:'/list',
              component:List
          }
      ]


  }
  ~~~

  ## 뷰 템플릿 작성시 주의사항
  - Vue 의 Template 에는 최상위 태그 1개만 있어야 렌더가 가능하다
  ~~~javascript
    var Foo={
        template:`
        <div>foo</div>
        <router-view></router-view>
        `
        //에러발생
    }
  ~~~
  - 여러개의 태그를 최상위 태그 레벨에 동시에 위치 시킬 수 없음
  - 따라서 아래와 같이 최상위 Element는 한개만 지정해야 한다.
   ~~~javascript
    var Foo={
        template:`
        <div>foo
          <router-view></router-view>
        </div>
        `
        //div 태그안에 텍스트와 router-view 포함하여 정상 동작 
    }
  ~~~

  ## Named views
  - 특정 url로 이동했을때 여러개의 View(컴포넌트)를 동시에 렌더링 하는 라우팅 방법
  - 각 컴포넌트에 해당하는 name 속성과 router-view 지정필요
  
  ~~~ html
  <div id="app">
   <router-view name="nestedHeader"></router-view>
   <router-view></router-view>
  </div>
  ~~~
  ~~~ javascript
  {
      path:'/home',
      //Named Router
      components:{
          nestedHeader: AppHeader,
          default: Body
      }

  }
  ~~~

  ## Nested View 와 Named Views 차이점
  - 특정 url에서 1개의 컴포넌트에 여러개의 하위 컴포넌트를 갖는것을 Nested
  - 특정 url에서 여러개의 컴포넌트를 쪼개진 뷰단위로 렌더링 하는것을 Named

## 참고사이트
https://joshua1988.github.io/web-development/vuejs/vuejs-tutorial-for-beginner/