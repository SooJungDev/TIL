## Helm 이란
Helm은 복잡한 k8s 리소스들을 간편하게 관리할 수 있도록 도와주는 툴

## helm 개념
- 차트(chart): 쿠버네티스 애플리케이션을 만들기위해 필요한 정보들의 묶음
- 컨피그(config) : 배포 가능한 객체들을 만들기 위해 패키지된 차트에 넣어서 사용할 수 있는 설정을 가지고 있음
- 릴리즈(release) : 특정 컨피그를 이용해서 실행중인 차트의 인스턴스 

## helm 구성요소
2개의 요소로 구성되어있음 
- helm client : 사용자가 직접 사용 할 수 있는 명령줄 도구 
- Tiler Server : 쿠버네티스 클러스터 내부에서 실행되어서 helm 클라언트에서 오는 요청을 처리하기 위해 대기


## 참고사이트
- [helm 기본](https://arisu1000.tistory.com/27860)
- [스포카에서 Helm을 도입 하는 이야기](https://spoqa.github.io/2020/03/30/k8s-with-helm-chart.html)