## Node.js

* Server-side 에서도 실행 가능한 자바스크립트 실행환경. Server side Language
* JS: 브라우저의 작은 단위 처리 -> Client Side FW -> Server Side Language
* HTTP 요청 서버코드를 직접 작성해주거나 Express 같은 웹 프레임워크를 사용

<pre><code>
// Http서버
 var http= require(‘http’);

http.createServer(function (req, res) {
    res.write(‘Hello World!’); //클라이언트에 보내는 응답
    res.end(); //응답 종료
}).listen(8080); // 로컬 호스트 8080 포트에서 서버 가동중
</code></pre>




<pre><code>
//Express 프레임워크
var express=require(‘express’);
var app =express();

app.get(‘/‘,function (req, res){
  res.send(‘Hello world!’); 
});

app.listen(8080, function(){
	console.log('Example app listening on port 3000!');
});
		
</code></pre>

## Node.js의 특징
 event-driven, non-blocking I/O model
  - Node.js 위에서 실행되는 API 는 모두 비동기 방식으로 실행된다. 
<pre><code>
  //server.js
  app.get(function(){
     //
  });

  app.post(function(){
    //
  });
</code></pre>  

 장점     
 1.v8 Engine이다. 
 구글 크롬 내장엔진 => 자바스크립트 엔진으로 실행됨. 

 2.Event -driven 방식 
 사용자가 이벤트를 발새시켰을때 , 즉 입력장치로 데이터를 전송했을때만 작동하는 방식. 
 발생한 이벤트에 대해서만 웹서버가 연결 해주기 때문에 자원을 최소 할 수 있음. 
 대부분의 웹서버는 사용자가 이벤트를 발생하기 까지는 기다리면서 자원을 최소 할 수 있다.
 대부분의 웹서버는 사용자가 이벤트를 발생하기 까지를 기다리면서 자원(대기시간/시간/메모리) 계속 소비함

 3.non-blocking
 blocking I/O 방식은 read/write 이벤트가 발생하면 이벤트가 끝날때까지 해당 모듈을 점유함. 
 즉 다른일을 할수 없고 메모리 버퍼에 데이터를 차지하게 되므로 메모리 소비. 
 요청이 완료 될 때 까지 Thread를 대기모드로 전환 시켰다 요청이 완료 된 후 유저 코드를 싱행시킴

 블로킹 방식의 비효울성을 극복하기위해 만든것이 non-blocking 방식
 I/O 를 진행하는 동안 유저 프로세스의 작업을 중단시키지 
 non-blocking 의 경우 read/write 이벤트가 시작하자마자 모듈을 변환시켜 다른작업을 하도록 준비상태가 됨.    
 그래서 속도가 빠르고 메모리도 덜차지하게됨. 

 4.Single thread
 적응양의 자원으로 일을 처리하는 것이 장점
 하지만 한곳에 예외 상황이 발생 할 경우 어플리케이션 전체가 영향이감

 가장큰 장점은 클라이언트와 서버간의 언어가 동일하다는점 

## Node.js 관련 라이브러리
 - 간단한 웹 애플리케이션을 구동하기 위한 설정 라이브러리 

## Express.js
 - Node.js에서 가장 많이 쓰는 웹 프레임워크로 간단하게 웹서버 구축가능
 - express.Router()을 이용하여 라우팅 기능 사용이 가능 


<pre><code>
 var express =require('express');
 var app =express();

 //GET 요청에 대한 라우팅
 app.get('/',function(req, res){
 	res.send('hello world');
 });

 //HTTP POST 요청에 대한 라우팅 
 app.post('/', funciton(req, res){
 	res.send('POST request to the hompage');
 });	
</code></pre>


## Body Pareser
 - Request  Body에 대한 요청을 파싱하여 req.body 에 자동으로 담아주는 Node.js 파싱 미들웨어

<pre><code>
 var bodyParser =require ('body-parser');
 //create application/json parser
 var jsonParser= bodyParser.json();

 //POST /api/user gets JSON bodies
 app.post('/api/users', jsonParser, function(req, res){
 	if(!req.body) return res.sendStatus(400)
 	//receive the request body in POST http request
 	console.log(req.body);
 })	
</code></pre>


 ## 실행시킬때 
 터미널에서 node 해당파일이름 ex) node server.js 
 하면 저절로뜸     
 vscode 에서 종료시킬때 터미널에서 ctrl+c 누름

 ## 참고사이트
 * 빠르게 배우는 Node.js와 NPM 설치부터 개념잡기 : https://kdydesign.github.io/2017/07/15/nodejs-npm-tutorial/  
 * node.js 이해하기 : http://programmingsummaries.tistory.com/328  
 * node.js 란 무엇인가? : http://asfirstalways.tistory.com/43  
 * node.js gizip 압축 적용: http://inspiredjw.com/entry/Expressjs%EC%97%90-Gzip-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0
 * node-http-server: https://mylko72.gitbooks.io/node-js/content/index.html
