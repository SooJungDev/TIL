## vue.js로 구현하는 PWA 캠프 정리

## Service Worker란?
- 브라우저와 서버 사이의 미들웨어 역할을 하는 스크립트 파일
- Offline Experience과 Mobile & Web Push의 기반술 

## Service Worker특징
1. <b>브라우저의 백그라운드에서 실행</b>되며 웹페이지와 별개의 라이프 사이클을 가짐
   - javascript UI 스레드랑 별도로 동작하는 또 다른 스레드 
2. 네트워트 요청을 가로챌 수 있어 해당 자원에 대한 캐쉬 제공 또는 서버에 자원요청.  
   - 프로그래밍 가능한 네트워크 프록시  
3. 브라우저 종속적인 생면주기로 백그라운드 동기화 기능 제공.  
   - push 알람의 진입점을 제공
4. web & mobile push 수신이 가능하도록 Notification 제공
5. navigator.serviceworker 로 접근
6. 기존 Javascript와의 별개의 자체 스코프를 가짐
   - 크롬 개발자 도구의 console과 별개의 서비스 워커전용 console 존재
7. DOM에 직접적으로 접근이 불가능 -postMessage()이용
8. 사용하지 않을대 자체적으로 종료, 필요시에 다시 동작(event-driven 방식)

## Service Worker등록
 - 브라우저에 존재 유무를 확인후 register()사용
~~~ javascript
if('serviceWorker' in navigator){
    //간단한 실행
    navigator.serviceWorker.register('/service-worker.js');
    //promise이용
    navigator.serviceWoker.register('/service-worker.js')
    .then(function(reg){
        //성공하면
        console.log('Okay it worked!',reg);
    })
    .catch(function(err){
        //실패해서 에러가 발생하면 
        console.log('Oops, an error occured',err);
    })
}
~~~

초기 렌더링에 방해되는 리소스는 최대한 뒤로 미룬다
~~~javascript
if('serviceWorker' in navigator){
    window.addEventListener('load',function(){
        //sw의 install에 필요한 자원 네트워크요청이 초기 렌더링에 필요한 자원 요청보다
        //나중에 일어나는것이 좋음 
        navigator.serviceWoker.register('/service-worker.js');
    })
}
~~~

- 서비스워커가 동작할 url scope 지정하기
~~~javascript
navigator.serviceWorker.register('/service-worker.js',{
    scope:'./myApp'
});
~~~
서비스워커의 위치는 주 도메인과 같은 위치에 있어야 합니다.

## Service Worker 설치
- register()에서 등록한 스크립트 파일에서 install()호출

~~~javascript
self.addEventListener('install', function(event){
 //캐쉬 등록 또는 기타로직 수행 
});
~~~


~~~javascript
var CACHE_NAME = 'cache-v1';
var filesToCache=[
    '/',
    '/js/app.js',
    '/css/base.css'
];
~~~

- CACHE_NAME:캐쉬를 담을 파일명 정의
- filesToCache:캐쉬할 웹 자원들 정의

~~~javascript
self.addEventListener('install',function(event){
    event.waitUntil(
        caches.open(CACHE_NAME).then(function(cache){
            //위에 지정한 캐쉬 목록을 'cache-v1' 캐쉬에 추가
            return cache.addAll(filesToCache);
        
        })
    );
});
~~~
   - waitUntil(): 안에 로직이 수행될때 까지 대기  
<b>캐쉬할 파일중 한개라도 실패하면 전체실 이를 해결하기 위해 sw-toolbox 사용가능</b>

## Service Worker 설치
1. self.addEventListener('install',fn) 로 서비스워커 설치
2. cache명 및 cache할 파일목록 지정
3. install 시에 위에서 지정한 캐쉬 등록
4. 개발자 도구로 서비스워커 설치 및 캐시 정상 등록 여부 확인 

## Service Worker 네트워크 요청 응답    
 - 서비스 워커설치 후 캐시된 자원에 대한 네트워크 요청이 있을때는 캐쉬로 돌려준다.
~~~ javascript
self.addEventListener('fetch',function(event){
  event.respondWith(
      caches.match(event.request)then.(function(response){
          if(response){
              return response;
          }
          return fetch(event.request);
      })
  );
});
~~~
- respondWith(): 안의 로직에서 반환된 값으로 화면에 돌려줌
- match(): 해당 request에 상응하는 캐쉬가 있으면 돌려주고 아니면 fetch()로 자원회득

## Service Worker캐쉬 응답
1. self.addEventListener('fetch',fn)로 캐쉬 응답 설정
2. event.respondWith()로 네트워크 요청 가로채기 및 캐쉬응답
3. 개발자 도구 Network패널의 offline 체크후 캐쉬응답 확인

## Service Worker 활성화 및 업데이트
- 새로운 서비스 워커가 설치되면 활성화 단계로 넘어온다.
- 이전에 사용하던 서비스워커와 이전캐쉬는 모두 삭제하는 작업 진행 

~~~javascript
self.addEventListener('activate',function(event){
    event.watiUntil(
        caches.keys().then(function(cacheNames){
            return Promise.all(
                cacheNames.mpa(function(cacheName){
                    //새로운 서비스 워커에서 사용할 캐쉬 이외의 캐쉬는 모두삭제
                    if(cacheWhitelist.indexOf(cacheName)==-1){
                        return caches.delete(cacheName);
                    }
                })
            );
        })
    );
});
~~~
기존에 실행중인 서비스 워커와 사이즈를 비교하여 1바이트라도 차이나면 새걸로 간주

## Service Worker 라이플 사이클
- 서비스워커는 웹페이지와 별개의 생명주기
- 서비스 워커 등록 & 설치 & 활성화 과정을 보면
  1. 웹페이지에서 서비스워커 스크립트 호출
  2. 브라우저 백그라운드에서 서비스워커 설치
  3. 설치과정에서 정적 자원 캐싱(캐시 실패시 install실패)
  4. 설치 후 활성화. 네트워크 요청에 대한 가로채기 가능
- 사용하지 않을때는 휴지상태. 필요시에만 해당 기능 수행
- 메모리 상태에 따라 자체적으로 종료하는 영리함

## Service Worker 디버깅
- 크롬 개발자 도구의 Application 패널에서 Service Workers
- 주소창에 chrome://inspect/#service-workers
- 주소창에 chrome://serviceworker-internals
- 주의사항: 등록 과정에서 실패하였더라도 콘솔에 로그 찍히지않는 경우가 생기니
    - event.waitIntil() 내부 콜백 로직 확인 
    
## 참고사이트
https://developers.google.com/web/fundamentals/primers/service-workers/
https://github.com/w3c/ServiceWorker/blob/master/explainer.md
https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage
https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage
