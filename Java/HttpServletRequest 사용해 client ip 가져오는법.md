

## HttpServletRequest 이란?
- http 프로토콜의 request 정보를 서블릿에게 전달하기 위한 목적으로 사용
- 헤더정보, 파라미터 쿠키, URI,URL 등의 정보를 읽어 들이는 메소드르 가지고있음
- Body의 stream을 읽어 들이는 메소드를 가지고 있다.



## HttpServletRequest client ip 가져오는 방법

- client ip 가져오는 방법
~~~ java
public static String getClientIP() {

    HttpServletRequest request = ((ServletRequestAttributes)RequestContextHolder.currentRequestAttributes()).getRequest();

    String ip = request.getHeader("X-FORWARDED-FOR");

    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("Proxy-Client-IP");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("WL-Proxy-Client-IP");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("HTTP_CLIENT_IP");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getHeader("HTTP_X_FORWARDED_FOR");
    }
    if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
        ip = request.getRemoteAddr();
    }

    // ipv6 (ipv4 127.0.0.1) localhost로 접속했을시
    if("0:0:0:0:0:0:0:1".equals(ip)){
        ip = "127.0.0.1";
    }
    
    return ip;

}
~~~



## 참고사이트
- [Interface HttpServletRequest](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html)
- [Request, Response 객체이해하기](https://andamiro25.tistory.com/148)
- [클라이언트IP 가져오기](https://blog.jin3260.com/m/71)
