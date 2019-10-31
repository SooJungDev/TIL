## sdkman
- SDKMAN 은 대부분 유닉스 기반 시스템에서 여러가지 SDK(Software Development Kits)의 병렬 버젼을 관리하기 위한 도구
- 여러 버전의 java 버젼을 관리 할 수 있음

## sdkman java 설치
~~~shell script
# 설치할 Java 버전 조회
$ sdk list java
================================================================================
Available Java Versions
================================================================================
 Vendor        | Use | Version      | Dist    | Status     | Identifier
--------------------------------------------------------------------------------
 AdoptOpenJDK  |     | 13.0.1.j9    | adpt    |            | 13.0.1.j9-adpt
               |     | 13.0.1.hs    | adpt    |            | 13.0.1.hs-adpt
               |     | 12.0.2.j9    | adpt    |            | 12.0.2.j9-adpt
               |     | 12.0.2.hs    | adpt    |            | 12.0.2.hs-adpt
               |     | 11.0.5.j9    | adpt    |            | 11.0.5.j9-adpt
               |     | 11.0.5.hs    | adpt    |            | 11.0.5.hs-adpt
               |     | 8.0.232.j9   | adpt    |            | 8.0.232.j9-adpt
               |     | 8.0.232.hs   | adpt    |            | 8.0.232.hs-adpt
 Amazon        |     | 11.0.5       | amzn    |            | 11.0.5-amzn
               |     | 8.0.232      | amzn    |            | 8.0.232-amzn
               |     | 8.0.202      | amzn    |            | 8.0.202-amzn
 Azul Zulu     |     | 13.0.1       | zulu    |            | 13.0.1-zulu
               | >>> | 12.0.2       | zulu    | installed  | 12.0.2-zulu
               |     | 11.0.5       | zulu    |            | 11.0.5-zulu
               |     | 10.0.2       | zulu    |            | 10.0.2-zulu
               |     | 9.0.7        | zulu    |            | 9.0.7-zulu
               |     | 8.0.232      | zulu    |            | 8.0.232-zulu
               |     | 8.0.202      | zulu    |            | 8.0.202-zulu
               |     | 7.0.181      | zulu    |            | 7.0.181-zulu
 Azul ZuluFX   |     | 11.0.2       | zulufx  |            | 11.0.2-zulufx
               |     | 8.0.202      | zulufx  |            | 8.0.202-zulufx
 BellSoft      |     | 13.0.1       | librca  |            | 13.0.1-librca
               |     | 12.0.2       | librca  |            | 12.0.2-librca
               |     | 11.0.5       | librca  |            | 11.0.5-librca
               |     | 8.0.232      | librca  |            | 8.0.232-librca
 GraalVM       |     | 19.2.1       | grl     |            | 19.2.1-grl
               |     | 19.1.1       | grl     |            | 19.1.1-grl
               |     | 19.0.2       | grl     |            | 19.0.2-grl
               |     | 1.0.0        | grl     |            | 1.0.0-rc-16-grl
 Java.net      |     | 14.ea.18     | open    |            | 14.ea.18-open
               |     | 13.0.1       | open    |            | 13.0.1-open
               |     | 12.0.2       | open    |            | 12.0.2-open
               |     | 11.0.2       | open    |            | 11.0.2-open
               |     | 10.0.2       | open    |            | 10.0.2-open
               |     | 9.0.4        | open    |            | 9.0.4-open
 SAP           |     | 12.0.2       | sapmchn |            | 12.0.2-sapmchn
               |     | 11.0.4       | sapmchn |            | 11.0.4-sapmchn


# 13.0.1-zulu 버전 설치
$ sdk install java 13.0.1-zulu
~~~
- 설치되면 아래 위치에 저장된다
~~~
~/.sdkman/candidates/java/
~~~

사용법은 아래 링크에 자세히 나와있음
- [SDKMAN Usage](https://sdkman.io/usage#installdefault)


## intellij 에서 sdkman 의 jdk 경로 설정
- 인텔리 제이에서 접근 하려면 Mac에서는 ./sdkman 이 기본적으로 숨김 폴더 경로 찾을 수 없음

- 아래와 같이 심볼릭 링크를 만들어서 접근하도록 해야함
~~~
ln -s ~/.sdkman ~/sdkman
~~~

- 그 다음 해당 경로로 세팅 

## 참고
- [sdkman](https://sdkman.io/)
- [SDK! 으로 Java 버전 관리하기](https://phoby.github.io/sdkman/)
- [(MacOS) IntelliJ 에서 SDKMAN의 JDK 경로 설정하기](https://cjred.net/2019-07-14-intellij-sdkman-jdk/)