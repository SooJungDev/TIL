## Minikube 란?
- Minikube는 쿠버네티스를 로컬에서 쉽게 실행하는 도구이다. 
- Minikube 는 매일 쿠버네티스를 사용하거나 개발하려는 사용자들을 위해 가상 머신(VM)이나 노트북에서 단일 노드 쿠버네티스 클러스터를 실행한다

## kubectl과 minikube 설정하기
1. Mac에서 kubectl 과 minikube를 세팅하기 위해서 brew를 활용해서 설치해줌
~~~
brew install kubectl minikube
~~~

2. minikuebe 에서 vm 을 사용하기 위해 virtualbox를 설치
~~~
brew cask install virtualbox virtualbox-extension-pack
~~~

3.설치가 완료되었으면 kubectl 명령어를 입력하면 다음과 같은 에러메세지가 출력(에러메세지 생략)
~~~
kubectl get services
~~~

4. minikube 실행
~~~
minikube start --vm-driver=virtualbox
~~~

5.kubectl 명령확인하기
- node 확인
- pods 확인
- service 확인
~~~
kubectl get node 
kubectl get pods
kubectl get services
~~~

## k8s 대시보드확인
- 컨테이너의 경우 CNI(Container Network Interface) 구조의 네트워크로 구성되어  있기때문에 외부에서 접속하기 위해서는
로컬 머신안에서도 프락시 설정해야됨 하지만 minikube의 경우 간단하게 확인할 수 있음
~~~
minikube dashboard
~~~

## minikube 가상머신 중지
~~~
minikube stop
~~~

## minikube 삭제하기
~~~
minikube delete
~~~

## 매니페스트파일
- k8s 에서 클러스터에 애플리케이션을 어떻게 디플로하고 클라이언트에게 엑세스를 어떻게 처리해야 할지에 대한 파일을 매니페스트 파일이라고함
- 매니페스트 파일안에는 자원할당과 네트워크 설정정보등을 담고있으며, k8s를 구동하기 위한 핵심파일이라고 할 수 있음
- 보통 YAML 파일을 통해 작성하게되며 매니페스트 파일은 다음 2가지로 나뉨
    - deployments
        - k8s에서 pod 관리와 네트워킹 목적으로 함께 묶여있는 하나이상의 컨테이너 그룹 pod의 헬스체크와 생성 및 스케일링 관리하는 역할
    - service
        - k8s의 pod는 컨테이너 내부의 네트워크 주소를 할당받기 떄문에 외부에서 접속하기 위해서는 해당 pod의 서비스를 외부 IP로 노출시켜야함
        - 이러한 정보를 담고있는것이 서비스
    
## minikube 클러스터를 활용한 hello-word
1. 미니쿠베를 실행시킴
~~~
minikube start
~~~

2. k8s 공식으로 제공하는 이미지파일을 통해 hello-node 라는 어플리케이션을 디플로이한다
~~~
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
~~~

3. 디플로이됬는지 확인
~~~
kubectl get deployment
kubectl get pods
~~~

4. k8s 클러스터안에 존재하는 pod 에 내부 IP주소로 할당되어있어서 외부에서 접속할수 없음
외부에서 접속하기 위해서 아래와 같이 설정
~~~
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
~~~

~~~
kubectl get services
~~~
- k8s 환경에서는 외부 ip 할당이 완료되지만 **minikube에서는 추가적인 명령해줘야**됨

추가적인 명령은 아래와같음
~~~
minikube service hello-node
~~~
- 명령어를 입력하면 디폴트로 설정된 웹브라우저가 나타나면서 Hello World! 가 표시된다

어플리케이션 삭제하는법
~~~
kubectl delete deployments hello-node
kubectl delete service hello-node
~~~

## 참고사이트
- 아래 사이트에 자세히 잘 설명해주셨음!!
- [Minikube로 쿠버네티스 설치](https://kubernetes.io/ko/docs/setup/learning-environment/minikube/#minikube-%ed%8a%b9%ec%a7%95)
- [Mac OS에서 minikube 사용하기 Part 1](https://judo0179.tistory.com/70?category=349244)
- [minikube를 활용한 간단한 애플리케이션 서비스하기 PART 2](https://judo0179.tistory.com/714)
