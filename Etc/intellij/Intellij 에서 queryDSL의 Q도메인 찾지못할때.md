## Intellij 에서 queryDSL의 Q도메인 찾지못할때
인텔리제이에서 Qdomain 이 존재하는 상황인데 빌드하면 컴파일에러가 남
src/main/generated 폴더안에 Qdomain 이 존재한다

generated 폴더를 패스에 추가해줘야됨
**File > Project Structure > Modules** 에 들어가고 우측에 src/main/generated 폴더를 source 로 추가해준다
사진은 아래 참고사이트에 자세히 저자분이 추가해주심
**그다음 빌드를 해주면 된다**

또한 다른 경우지만 Querydsl 의 QClass 를 담는 src/main/generated 는 자동으로 생성되는 파일들의 디렉토리니 무조건 **.gitignore 에 추가해야함**


## 참고사이트
- [Intellij 에서 queryDSL의 Q도메인 찾지못할때](https://blusky10.tistory.com/275)
- [Spring Boot Data Jpa 프로젝트에 Querydsl 적용하기](https://jojoldu.tistory.com/372)
