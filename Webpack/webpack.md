# Webpack - Module Bundler

## Webpack 이란?
- 서로 연관 관계가 있는 웹자원들을 js,css,img 와 같은 스태틱한 자원을 변환해주는 모듈 번들러
- 모듈번들러란 각각의 모듈들에 대해 의존성관계를 파악하여 그룹핑해주는것 

Module with dependencies -> webpack(module bundler) ->static assets

## 사용하는 이유와 배경
1. 새로운 형태의 web task manager


- 모든 웹자원 (js.css,html)이 모듈 형태로 로딩가능
- 초기에 불필요한 것들을 모두 로딩하지 않고, 필요할때 필요한 것만 로딩하여 사용

## webpack Entry
- webpack 으로 묶은 모든 라이브러리들을 로딩할 시작점 설정
- a,b,c 라이브러리를 모두 번들링한 bundle.js 를 로딩한다.
- 1개 또는 2개 이상의 엔트리 포인트를 설정 할 수 있음

## webpack Output
- entry에서 설정하고 묶은 파일의 결과값을 설정

## webpack Loader
- 웹팩은 자바스크립트 파일만 처리가 가능하도록 되어있다.
- loader 를 이용하여 다른 형태의 웹자원 (img,css,...)js로 변환하여 로딩