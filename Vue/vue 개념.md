# 시작하기

선언적 렌더링
Vue.js 간단한 템플릿 구문을 사용해 선언적으로 DOM에 데이터를 렌더링 하는것

## Vue.js가 뭔가?
작은 화면단 라이브러리 역할부터 큰규모의 웹어플리케이션 개발을 돕는 프레임워크 역할
점진적으로 적용할수 있는 프런트엔드 프레임워크

특징 3가지
- 1.컴포넌트 기반 개발방식
=> 화면을 여려개의 작은단위로 쪼개어 개발
    재사용성 구현속도 코드가독성이 높아짐
- 2.MVVM 패턴
화면 UI코드와 백엔드 데이터 처리 코드를 분리 
- 3.리액트와 앵귤러의 장점을 흡수
Two-way data binding, Virtual DOM


Vue.js 구성요소
Vue instance
Vue component
Vue Router
Vue Template

## 1.뷰 인스턴스
New Vue({
          el : //인스턴스가 뿌려질 지점
          template://인스 턴스의 화면 내용 
          data: //인스턴스가 갖는 데이터 
         methods: //인스턴스의 이벤트정의 
         created: //인스턴스 라이플 사이클
});

뷰로 화면을 개발할때 필수로 생성해야 하는 단위

## 2.뷰 컴포넌트
화면을 구조적으로 설계하기 위한 요소
Vue.compont(‘my-cmp’,{  
  //인스턴스 옵션 속성과 동일
    template://인스턴스 화면내용 
    data://인스턴스가 갖는 데이터 
    methods://인스턴스의 이벤트 정의
    created : //인스턴스 라이프사이클
});

## 3.뷰라우터
여러개의 화면간에 이동하는 방법
var Main = {template: '<div>main</div>'};
var Login ={template:'<div><login/div>'};

var router =new VueRouter{{
    routes : [

        {path:'/main',component:Main},
        {path:'/login',component:Login}

    ]

});

var app =new Vue({
    el:'#app',
    router:router
});

## 4.뷰 템플릿
화면을 구체적으로 꾸미는 방법&문법
{{message}},
{{message.split('').reverse().join('')}}
<p v-bind:id="uid">인스턴스의 값과 연결</p>
<a v-if="seen">seen 값에 따라 표시 여부 결정</a>
<ul v-for="item in items">{{item.name}}</ul>
<button v-on:click="pAlert">클릭하면 경고창 표시</button>

뷰 인스턴스 생성 -> 뷰 컴포넌트 설게 및 생성 -> 뷰 컴포넌트 내용구현

vue.js 구현절차
Vue instance -> Vue Components -> Vue Template


특정 속성에 반응성을 주입하는 코드(getter/setter)
화면이 다시 그려지는 과정 -> 뷰 인스턴스 객체 속성 watcher

변경 내용을 추적하는 방법
Vue 인스턴스에서 javaScript 객체를 data 옵션으로 전달하면 vue는 모든 속성에 
Object.defineProperty를 사용하여 getter/setter로 변환합니다. 이는 vue가 ES5를 사용 할 수 없는 IE8이하를 지원하지않는 이유이다.
![2018-06-07 4 21 46](https://user-images.githubusercontent.com/38197944/41890524-d48dfa7c-794a-11e8-992d-60a75f5e4d6e.png)

getter/setter 는 사용자에게 보이지 않으나 속성에 엑세스 하거나 수정할때 뷰가 종속성 추적 및 변경 알림을 수행 할 수있음
모든 컴포넌트 인스턴스에는 해당 watcher 인스턴스가 있으며 이 인스턴스는 컴포넌트 종속적으로 렌더링 되는 동안 “수정”된 모든 속성을 
기록합니다. 나중에 종속적인 setter가  트리거 되면 watcher에 알리고 컴포넌트가 다시 렌더링됩니다. 
![2018-06-07 4 27 41](https://user-images.githubusercontent.com/38197944/41890612-3718fa3e-794b-11e8-8fa8-cd5e1c39b800.png)


## 참고링크
https://www.slideshare.net/GihyoJoshuaJang/do-it-vuejs-88453012


