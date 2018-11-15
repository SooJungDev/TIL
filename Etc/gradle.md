## 빌드 자동화란?
- 프로젝트에서 개발하기전에 로컬 개발에서 환경 세팅을 함 보통 먼저 하는일은 빌드 환경 설정이다  
 정적인 타이피 언어는 보통 코드를 작성하면 컴파일을 해서 오브젝트 파일을 생성하고, 
 링킹이라는 작업을 통해서 실행파일 또는 java의 jar와 같은 라이브러리 파일을 생성한다.  
 언어마다 차이가 있지만 이런 작업들을 자동화 하는것이 빌드 자동화이다.

## gradle의 특장점
- 간결함
- 속도 
- 멀티프로젝트 구성
- 유연성 + 확장성 
- 플러그인 생태계

## 기본 설정
gradle init 명령어를 치면 아래 폴더 및 파일들이 생성
~~~
project/
    gradlew
    gradlew.bat
    gradle/wrapper/
        gradle-wrapper.jar
        gradle-wrapper.properties
    build.gradle
    settings.gradle
~~~
- gradle wrapper : 사용하는 목적은 이미 존재하는 프로젝트를 새로운 환경에 설치할때 별도의 설치나 설정과정없이 바로 빌드하기 위해
- gradlew 파일 : 유닉스용 실행 스크립트
- gradlew.bat 파일: 윈도운용 실행 배치 스크립트
- gradle/wrapper/gradle-wrapper.jar 파일: Wrapper 파일 gradlew 나  gradlew.bat 파일이 프로젝트 내에 설치하는 이파일을 사용하여 gradle task를 실행하기 때문에 로컬 환경의 영향을 받지 않음   
(실제로는 Wrapper 버전에 맞는 구성들을 로컬캐시에 다운로드 받음)
- gradle/wrapper/gradle-wrapper.properties 파일 : Gradle wrapper 설정 파일임, 이 파일의 wrapper 버전등을 변경하면 task 실행시 자동으로 새로운 Wrapper 파일을 로컬 캐시에 다운로드 받는다 
- build.gradle 파일 : 의존성이나 플러그인 설정등을 위한 스크립트 파일
- settings.gradle 파일 ; 프로젝트의 구성 정보를 기록하는 파일. 
  어떤 하위프로젝트들이 어떤 관계로 구성되있는지 기술 . gradel은 이파일에 기술된대로 프로젝트 구성

## 참고사이트
  - [빌드 자동화 by Gradle](https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09)
  - [구글](https://www.google.com/)
  - [구글](https://www.google.com/)