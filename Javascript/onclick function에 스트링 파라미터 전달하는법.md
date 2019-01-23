## onclick function 에 스트링 파라미터 전달하는 방법 
-밑에와 같이 전달했을때 숫자는 이상없었는데.. 스트링은 에러가 떨어져 가지지 않았음
~~~
'<input type="button" onClick="gotoNode(' + result.name + ')" />'
~~~
아래와 같이 해줘야함 
~~~
'<input type="button" onClick="gotoNode(\'' + result.name + '\')" />'
~~~
멀티로 보내는법!!!
stackoverflow 에서 참고했음
~~~
   bounds.extend(marker.position);
        bindInfoWindow(marker, map, infowindow,
     '<b>' + response[i].driver_name + '</b><br>' + 
     '<b>' +moment(response[i].updated_at).fromNow() + '</b>
      <button onclick="myFunction(\''+response[i].id+'\',\''+driversList+'\')">Click   me</button>'
    );
~~~
## 참고사이트
  - [stackoverflow pass string parameter in an onclick function](https://stackoverflow.com/questions/9643311/pass-string-parameter-in-an-onclick-function)
