## NPM 이란
자바스크립트 프로그래밍 언어를 위한 패키지 관리자. 
자바스크립트 런타임 환경 Node.js의 기본 패키지 관리자이다.  

## NPM(Node Package Manager)
* Js 개발자들이 편하게 개발할수 있도록 js 라이브러리들을 모아 놓은 열린공간. 
* Front-end web apps, mobile apps, robots, router 라이브러리들이 존재. 
* Gulp, Web pack 모두 node 기반, NPM을 사용하여 필요한 라이브러리들을 로딩. 
* 재사용 가능한 code 를 module, package 라고 호칭. 
* Package.json: 해당 package 에 대한 파일 정보가 들어가있음   
* keyword로 패키지 검색이 가능. 


## NPM 명령어 모음
Package.json 새성
*  프로젝트의 node module 들 관리를 위한 package.json 설정
*  -y를 붙이면 default 정보를 가지고 package.json 생성
  
<pre><code>
	npm init   
	npm init -y
</code></pre>

패키지 설치 및 업데이트
* 프로젝트에서 사용 할 node module 설치 및 package.json 업데이트
 
npm install 패키지명 —save. 
npm I 패키지명 —save. 

Global 설치 vs Local 설치. 
  
<pre><code>
	npm install gulp -global
	npm install gulp 
</code></pre>


- local 로 설치할경우 ./node_modules 밑에 설치되고 소스 내에서 require()로 불러들이는 모듈은 local로 설치한다
- global 로  설치할 경우 /usr/local 밑에 설치되고 터미널에서 모듈의 명령어를 사용할 일이 있다면 global로 설치한다.


//설치 안될경우 관리자 권한으로 변경해줘야함 앞에  sudo를 붙여준다.
sudo npm install gulp-cli -global      

Install —save vs —save-dev. 

* —save는 앱이 구동하기 위해 필요한 모듈 & 라이브러리 설치 ex) react, vue
  
<pre><code>
	//package.json
“dependencies”:{
    “vue”:”^2.3.3”
    ...
},
</code></pre>


*  —save -dev는 앱 개발 시 필요한 모듈& 라이브러리 설치 ex)test,build tool, live reloading
  
<pre><code>
	// package.json
“devDependencies”:{
    “gulp”: “^3.9.1”,
    ….
}
</code></pre>

![2018-06-26 3 51 39](https://user-images.githubusercontent.com/38197944/41894383-ad62e32c-7959-11e8-98d9-ad8c7667dbe8.png)
