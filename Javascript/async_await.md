# async (비동기 함수)
- 비동기 함수를 사용하면 메인스레드를 차단하지 않고도 동기 함수 인 것 처럼 프라미스 코드를 작성
  
~~~javascript
async function myFirstAsyncFunction(){
    try{
        const fulfilledValue = await promise;
    }catch(rejectdValue){
        //...
    }
}
~~~

- 함수 정의 앞에 async 키워드 사용하면 함수내에 await 사용 가능  
- promise를 await할 때 함수는 promise가 결정될때 까지 방해하지 않는 방식으로 일시중지
- promise가 이행되면 값을 돌려 받음
- 거부되면 thow 반환

## url을 가져와서 응답을 텍스트로 로그하는 경우
- promise를 사용하는 방법
~~~javascript
function logFetch(url){
    return fetch(url)
    .then(response => response.text())
    .then(text=>{
        console.log(text);
    }).catch(err => {
        console.error('fetch failed',err);
    });
}


~~~

- async 사용 콜백이 사라짐
~~~javascript
async function logFetch(url){
    try{
        const response =await fetch(url);
        console.log(await response.text());
    catch(err){
        console.log('fetch failed',err);
    }
}
~~~
* 참고 await 하는것은 전부 Promise.resolve()를 통해 전달 기본 프라미스가아닌 프라미스를 안전하게 await 할 수 있음
  
## 비동기 반환값
- await 사용여부와 상관없이 비동기 함수는 항상 프라미스를 반환

~~~javascript
//wait ms milliseconds
function wait(ms){
    return new Promise(r => setTiout(r,ms));
}

async function hello(){
    await wait(500);
    return 'world';
}
~~~
hello()를 호출하면 "world"와 함께 이행되는 promise가 반환

~~~javascript
async function foo(){
    await wait(500);
    throw Error('bar');
}
~~~
foo()를 호출하면 Error('bar')와 함께 거부되는 promise가 반환

## 참고사이트
https://developers.google.com/web/fundamentals/primers/async-functions?hl=ko
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function
