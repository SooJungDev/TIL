## Page speed insight 

- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=ko)

구글 웹페이지 속도 측정하는 사이트인데 내가 지금 현재 개발하고 있는 사이트의 점수를 높이기 위해 개선하는 작업을 했었다.



## iframe -> video 태그로 변경해주기
iframe 으로 youtube 링크를 불러오면 youtube 관련된 js 파일이 로딩이 아주느린현상이 있었다.
video 태그를 사용해서 자체적으로 불러오는것이 더빨랐다

브라우저마다 다름으로 video 태그 들어가는 영상은  mp4,webm 두가지 종류가 필요하다.
~~~
 <source src="영상경로" type="video/mp4">
 <source src="영상경로" type="video/webm">
 ~~~

## 효율적으로 이미지 인코딩하기

https://web.dev/uses-optimized-images/?utm_source=lighthouse&utm_medium=unknown
https://web.dev/use-lazysizes-to-lazyload-images/
https://github.com/aFarkas/lazysizes


일단은 효율적으로 이미지를 인코딩 하기위해서 여러가지 방법을 제공하는데 그중에 lazy loading 을 함
위의 사이트에서 추천하는 lazysizes 라이브러리를 사용함


~~~
아래와 같은 사용법으로 사용할 수 있으며
<img  data-src=“이미지 경로” class="lazyload" alt="">


Lazy loaded 가 되었을때는 아래 클래스가 lazyloaded 로 변경됨
<img data-src=“이미지경로”  class="lazyloaded" alt="" >
~~~

## 이미지 최적화
- 이미지 파일이 크면 클수록 느려짐
- 이미지 파일을 최대한 작게끔 PageSpeed Insights 사이트에서 권장하는 이미지 확장자를 사용하는것이좋음

## Eliminate render blocking css
- 아래와 같은 방법으로 개선함
~~~
<link rel="preload" as="style" href="css 경로"  onload="this.rel='stylesheet'">
~~~