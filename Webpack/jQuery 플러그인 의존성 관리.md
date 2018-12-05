## webpack의 jQuery 플러그인 의존성 관리
- jquery에 대한 전역 액세스를 하기위해서 설정해줘야함
- 일단 npm i jquery  jquery를 설치해줌
- webpack.base.config.js 파일에 추가해준다. 밑에 설정을
- plugins 는 module.exports 안에다가 넣어줌
~~~ javascript
const webpack = require('webpack');

 plugins: [
    new webpack.ProvidePlugin({
      $: "jquery",
      jQuery: "jquery"
    })
  ]
~~~
- 다시 빌드 

## 참고사이트
  - [webpack의 jQuery 플러그인 의존성 관리하기](https://code.i-harness.com/ko-kr/q/1ba0b85)
