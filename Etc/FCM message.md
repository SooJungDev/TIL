## Firebase 클라우드 메시징
- FCM(Firebase cloud messaging) 은 무료로 메시지를 안정적으로 전송 할 수 있는 교차 플랫폼 메시징 솔루션 
- FCM 을 사용하면 새이메일이나 기타 데이터를 동기화 할수 있음을 클라이언트 앱에서 알릴 수 있음 
- 

## 메세지 유형(알림메세지 vs 데이터메세지)
- 알림메시지
  - FCM 클라이언트 앱을 대신해 최종 사용자 기기에 자동으로 메세지 표시
  - 사용자에게 표시되는 키모음이 사전 정의되어 있음 
  - 전송방법 
    - 앱 서버 및 FCM 서버 API 사용: notification 키를 설정. 선택사항으로 데이터 페이로드 추가기능, 항상 축소형
    - 알림 콘솔 사용: 메세지 본문, 제목등을 입력하고 전송. 알림콘솔에서 맞춤데이터를 제공하여 선택사양인 데이터 
- 데이터 메시지 
    - 클라이언트 앱이 데이터 메세지 처리를 담당
    - 데이터 메시지에는 맞춤 키-값 쌍만 있음
    - 전송 방법
      - 앱 서버 및 FCM 서버 API 사용 : data 키만 설정, 축소형, 비축소형 모두 가능
      - ios에서 사용하기 위해서는 data 키에 맞춤 키-값 쌍을 설정하고 content_availavle true로 설정

## 개발자 모드로 실행하기
HttpHeader 에  "dry_run" 옵션을 추가해줘서 던져준다 dry_run true 이면 테스트로 전송됨
false 로 주면 진짜로 전송

## 참고사이트
  - [firebase 클라우드 메시징](https://firebase.google.com/docs/cloud-messaging/)
  - [알람메세지 vs 데이터 메세지](http://pliss.tistory.com/116?category=699640)
