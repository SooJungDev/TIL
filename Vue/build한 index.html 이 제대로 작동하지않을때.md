## build한 dist/index.html이 제대로 작동하지 않을떄
- vue cli 프론트 앤드 앱을 백엔드와 별도로 개발하는 경우 프론트 엔드는 기본적으로 완전히 정적인 앱
= dist 디렉토리에 빌드된 내용을 정적 파일 서버에 배포할 수 있지만 올바름 publicPath를 설정해야함 
- publicPath란?? 파일을 사용할 때 경로 앞에 추가되는 문자열이다. 
- 이 그래서 로컬에서 dist/index.html 통해 직접 열면 작동하지 않음. 프로덕션 빌드 로컬에서 미리 볼 수 잇는 가장 쉬운 방법은 node.js 정적 파일 서버를 사용하는것

~~~
npm install -g serve
serve -s dist
~~~

- 만약에 해당 index 파일이 정적이고 서버가 필요 없을때 열수있게하려면
1. vue.config.js 파일 (프로젝트 바로 밑에있어야함)에 baseUrl: ''로 설정해줌
~~~ javascript
module.exports = {
  baseUrl: '',
}
~~~
2. 두번째 vue-router를 사용하면 history mode를 사용하면 안되고 기본값 hashmode로..
3. 그래도 파일이 제대로 보이지 않고 밑에 같은 에러가 뜬다 해당 에러난  vue파일에 빈 <script></script> 가 있다면 다지워준다.
~~~
componentNormalizer.js:24 Uncaught TypeError: Cannot set property 'render' of undefined
    at i (componentNormalizer.js:24)
    at Module.56d7 (Footer.vue:9)
~~~

## 참고사이트
  - [vue cli 가이드](https://cli.vuejs.org/guide/deployment.html#general-guidelines)
  - [stackoverflow vue cli build and run index.html file without server](https://stackoverflow.com/questions/50809987/vue-cli-build-and-run-index-html-file-without-server)
  - [dist folder not working "Uncaught TypeError: Cannot set property 'render' of undefined](https://github.com/vuejs/vue-cli/issues/2430)
  