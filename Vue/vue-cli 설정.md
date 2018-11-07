## vue-cli 로 프로젝트 구성
- node.js 가 설치 되어있다고 가정
- npm install vue-cli -g (global 로 설치해줌)
  - 설치가 안될 경우 앞에 sudo 추가해서 설치
- 설치한 다음에 vue-cli 로 프로젝트 생성하고 초기 설정 자동으로 할수 있음
~~~ shell
$ vue init <template-name> <project-name>
~~~
- template 에는 여러종류가 있음 
  - webpack : hot-reload , linting, 테스트 및 css 추출 기능등 대부분의 기능을 갖추고 있는 webpack+vue-loader 설정
  - webpack-simple : 단순히 webpack 과 vue-loader만 포함. 빠르게 프로토 타입을 만들때 사용
  - browserify : hot-reload, linting, 단위테스트 및 대부분의 기능을 갖추고 있는 Browserify+vueify
  - browserify-simple : 단순히 Browserify+ vueify만 포함. 빠르게 프로토타입 만들때 사용 
  - pwa : PWA 템플릿 webpack으로 되어있음
  - simple : 단순하게 html파일에 vue 설정 
 - 간단하게 webpack 설정을 빨리하려면
 ~~~
 $ vue init webpack
 
 ? Vue build standalone
 ? Install vue-router? Yes
 ? Use ESLint to lint your code? Yes
 ? Pick an ESLint preset Airbnb
 ? Set up unit tests Yes
 ? Pick a test runner karma
 ? Setup e2e tests with Nightwatch? Yes
 ? Should we run `npm install` for you after the project has been created? (recommended) npm
 ~~~
 
 ## 개발환경에서 api proxy 설정
 - config/index.js 에 있는 dev.proxyTable 옵션을 수정 
 - proxyTable 밑에 옵션을 더넣어줌
~~~
module.exports = {
  dev: {
    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      "**": {
        target: 'http://localhost:8080',
        changeOrigin: true,
      }
    },
 ~~~
 위와 같이 수정해준다음에 밑에 명령어를 쳐준다
 ~~~
 npm run build --watch
 ~~~
위에 명령어를 친다음에 npm run dev 해주면 반영됨



## 참고사이트
  - [vue-cli 설정](https://jinblog.kr/192)
  - [npm vue-cli](https://www.npmjs.com/package/vue-cli)
  - [webpack 템플릿으로 개발환경 구축하기](http://itstory.tk/entry/vuecli-Webpack-%ED%85%9C%ED%94%8C%EB%A6%BF%EC%9C%BC%EB%A1%9C-vuejs-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)