## 공부하는 이유!
- 요새 기본기가 부족하다것을 많이 느낀다.ㅠㅠ 
- 아는것도 차근차근 정리해 볼 필요가있음.
- 생활코딩 리눅스 강좌듣고 리눅스 기초에대해서 다잡을 필요가 있다!
- 열심히 들어보자~~

##환경설정
- codeonweb
- ubuntu
- 등등
## 디렉토리와 파일
- ls : 현재 디렉토리의 파일 목록을 출력하는 명령어 ls -l은 자세히 보기
    - 자세히 보기 시 d가 붙어있으면 디렉토리 아니면 - 로시작
    - ls로 파일목록을 봤을 때 .으로 시작하는것들은 감춰진 파
- pwd : 현재 위치하고 있는 디렉토리를 알려주는 명령어
- mkdir : 새로 생성할 디렉토리명
 - mkdir -p dir1/dir2/dir3/dir4 
    - -p 옵션을 주면 여러개의 디렉토릐를 한꺼번에 만들 수 있
- touch empty_file.txt :empty_file.txt  의 빈파일이 만들어짐
- cd : cd 이동할 디렉토리의 경로명

상대경로 
- 상대경로는 현재 디렉토리의 위치를 기준으로 다른 디렉토리의 위치를 표현하는것으로 
- ..은 부모 디렉토리를 의미함
- cd .. 은 현재 디렉토리의 부모 디렉토리로 이동하는 명령
- 참고로 현재 디렉토리는 .

절대 경로
- 최상위 디렉토리 기준으로 경로를 표현하는것을 의미
- 최상위 디렉토리는 루트 root 디렉토리라고 하고 '/'이다
- cd / 최상위 디렉토리로 이동
- cd /home/crystal 현재 디렉토리가 무엇이건 언제나 /home/crystal 의미

rm 삭제
- rm 파일명
- rm -r 디렉토리명

--help (mac에서 안됨)
명렁어 뒤에 --help를 붙이면 명령의 사용설명서가 출력
- ls --help
- rm --help
- mkdir --help

man 명령어도 마찬가지
- 검색해서 보고싶을때 '/'
    - /sort
- 검색된것에서 다음 검색결과로 넘어가고싶을때 N을 누름
- Q를 누르면 밖으로 나오게

## 필요한 명령 검색방법
- ex) create directory in linux

## sudo (super user do약자)
- unix 계열 예전에는 하나의 컴퓨터를 여러사용자가 사용했음
- permission 을 다르게 줬음
- super user 또는 root user
    - rm -rf / 
        - 다삭제해라 너무위험함
- 관리자의 권한으로 실행할수 있게 해줌 sudo 명령어를 통해서

~~~
sudo apt-get install git
~~~

## nano 에디터 사용하기
- vi 도있지만 초급자들 nano 사용하기 편함
- ^(control)을 의미
- vi를 더추천함

## 패키지 매니저 homebrew

사용법 알고싶으면 
~~~
brew help
~~~

- htop 을 찾는다
~~~
brew search htop
~~~

- htop을 설치한다
~~~
brew install htop
~~~

- 삭제하기전에 정확하게 알고있어야함
- 리스트를 알수있음
~~~
brew list
brew uninstall htop
~~~

- 이미 설치한 프로그램을 upgrade 하고싶다면
~~~
brew upgrade vim
~~~

- brew update
~~~
brew update
~~~

## 다운로드 방법 -wget
~~~
wget 다운로드받을 링크
~~~

~~~
wget -O 저장하고싶은 이름 다운로드받을 링크
~~~

## 다운로드방법 -git
- 버전관리 시스템
- 형상관리를 하기 쉽게 해줌

- 디렉토리명에 프로젝트소스를 복제함 
~~~
git clone 프로젝트url 디렉토리명
~~~

## IO Redirection 
- Input
- Output
- Redirection : 방향을 돌려서 다른것들로 출력 해주는것

output  
- ls -l 의 결과를 result.txt 에 저장시킴
~~~
ls -l > result.txt
~~~

- > standard output 1>  (표준출력을 의미 1생략할수있음)

- standard error 2> 의미함 에러를 저장할때는 앞에 2를 붙여줌
~~~
rm rename2.txt 2> error.log
~~~


input  

- 표준 입력으로 받은 경우
~~~
cat < result.txt
~~~

- 기본 열줄만 출력해줌
~~~
head linux.txt
~~~

- 한줄만 출력해줌  command-line argument 를 준것
~~~
head -n1 linux.txt
~~~

- 밑은 standard input 을 준것
~~~
head -n1 < linux.txt 
~~~

- one.txt 에 해당 결과를 저장해라 
- 표준입력,출력에 대한 redirection을 모두함
~~~
head -n1 < linux.txt > one.txt 
~~~

append

- >> 가 두개면 결과를 append 해서 추가해줌 덧댄다.
~~~
ls -al >> result.txt
~~~

- 여러개의 입력을 하나로 합친다
~~~
mail egoing@gmail.com <<eot
hi
my
name
is
egoing
eot
~~~
- eot 가 들어가면 입력이 끝나는것임 << 잘안씀

- 낭떨어지 같은곳 아무것도 나오지않음 
~~~
ls -al > /dev/null
~~~

## 쉘과 쉘스크립트
- 운영체제에서 중심이되는 kernel
    - hardware 를 제어함
- 커널 바깥쪽을 감사고 있는 shell
    - 사용자가 명령어 입력한것을 kernel 에 전해주고 제어해줌

### bash vs zsh   
~~~
echo $0
-bash
~~~

cd 하고 tab 
- zsh: 숨김파일은 안나옴
- bash: tab 두번눌러야하고 전부다 나옴

cd 하고 경로
- zsh: cd /h/u 치고 tab 하면 경로가 자동으로되줌 cd why dir1으로 하면 dir1으로감 

커널과 쉘은 분리되어있기 때문에 자기에게 맞는 쉘을 선택해서 사용하면됨 

### Shell script 소개
- 여려개의 명령을 순차적으로 실행하는것 명령의 순서 각본 스크립트를 어딘가에다가 적어놓고 재사용 할수 있게 하는것 

~~~
mkdir script
cd script/
touch a.log b.log c.log
ls -l
~~~

- backup 파일을 만든다.
~~~
mkdir bak
cp *.log bak
ls -l bak
~~~

- 두번 실행 할 경우 에러가 나기떄문에 있다면, 만들고 없다면 만들지않는다
- 자주일어나는 작업이라고 생각해야함 왜쓰는가에 공감해야함!

### shell script 사례

~~~
nano backup
~~~

~~~shell
#!/bin/bash
if ! [ -d bak ]; then
       mkdir bak
fi
cp *.log bak       
~~~

- ctrl+x 누르고 y 저장해줌

~~~
./backup
chmod +x backup
~~~
- 실행 기능을 추가해준다
- x 표시가 추가되어있음 그뜻은 실행 가능하다는 뜻임 

~~~
./backup
~~~
- 실행시킨다.

## 디렉토리 구조와 파일 찾는법

### 디렉토리 구조 
- bin : user binaries
- sbin : system binaries
- etc : configuration files
- proc : process information
- var : variable files
- tmp : temporary files
- usr : user programs
    - usr/bin usr/sbin 등등 구분되어있음 
- home: home directories
    - 사용자의 디렉토
- opt : Optional add-on Applications

### 파일찾는법 - locate 와 find

- database 에서 찾음
- 하루에 한번 해당정보를 올려줌
~~~
locate
sudo updatedb
~~~

- find 실제로 뒤짐
- 옵션이 많기때문에 구글에서 찾아서 사용
~~~
sudo find / -name *.log 
~~~

- 검색된 파일을 삭제한다
~~~
find . -type f -name "tecmint.txt" -exec rm -f {} \;
~~~

### 파일찾는법 - whereis 와 $PATH
- 실행파일을 찾아줌
~~~
whereis ls
~~~

~~~
echo $PATH
~~~

## 프로세스와 실행

### 컴퓨터의 구조

### 프로세스 모니터링(ps, top, htop)

- ps 
    - 프로세스 리스트를 보여줌
    
- 백그라운드에 있는 것을 다보고 싶다면  aux 옵션을 주면됨
~~~
ps aux
~~~

- 이 중에서 apache 를 찾고 싶으면 
    - PID 식별자
~~~
ps aux | grep apache
~~~

- 프로세스를 죽이고싶으면
~~~
sudo kill pid
~~~

프로세스 목록보고싶으면 
~~~
sudo top
~~~

좀더 이뿌게 보여줄려고 htop 을 깔아줌
~~~
sudo htop
~~~
- Load average : 프로세스 점유율과 관련된것 3가지
    - 첫번째 1분간의 cpu , 5분, 15분 평균치를 내린것
    - 코어 숫자가 달라지면 의미가 달라


## SSH
- 원격제어를 사용할때 씀
- 서버와 클라이언트 구조가 필요함
- 서버 (SSH server)
- 클라이언트 (SSH client)
    - ssh 서버가 설치되어있는 컴퓨터를 제어하게됨

- 삭제하는법 (purge 강력한 삭제)
~~~
sudo apt-get purge openssh-server openssh-client
~~~

- 설치하는 법
~~~
sudo apt-get install openssh-server openssh-client
~~~

- 시작하기
~~~
sudo service ssh start
~~~

- 확인하기
~~~
sudo ps aux | grep ssh
~~~

- 접속하기
~~~
ssh egoing@192.168.0.65
~~~

ssh public private key

~~~
ssh-keygen
~~~
- enter 를 친다.
- 비밀번호를 입력해주면 더안전 입력하지 않아도됨
- .ssh 디렉토리가 생성 , 공개키 비공개키가 만들어
- cd .ssh
- id_rsa (private key)
- id_rsa.pub (public key)


- cd .ssh
- authorized_keys  퍼블릭 키를 로그인 하고싶은 서버 파일 끝에 붙여줘야함
    - ssh-copy-id egoing@192.168.0.67 (자동으로 해)



## domain
- google.com(도메인주소) 에 접속함
- DNS (domain name server): 도메인 네임으로 접속을하면 dns 서버의 ip로 접속을함 구글 닷컴에 아이피가 뭐냐?
- 구글닷컴에 아이피를 돌려주고 그 아이피 주소로 접속을하게함

hosts
- 파일
- 어떤 도메인이 어떤 ip를 가지고 있는지 알 수 있는 파일
- 도메인 네임서버가 등장하기 전에는 hosts 파일에있는 정보를 가져와서 접속
- /etc/hosts 파일을 변경했음
    - host 들의 ip들을 적어놓은 파일임
- hosts 파일에 해당 도메인주소가 있으면 dns 서버에 접속하지 않음
- DNS 서버가 등장하기 전부터 있었음. ip주소를 기억하기 어려움으로..
- 해커들의 공격 대상이 되기도함
    - 구글닷컴이랑 똑같이만듬
    - 공격자의 ip로 변조함
    - 로그인 시도하면 로그인 아이디, 비밀번호를 가져가서 탈취함
    
    

## 참고사이트
- [생활코딩 리눅스 강좌](https://www.inflearn.com/course/%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9-%EB%A6%AC%EB%88%85%EC%8A%A4-%EA%B0%95%EC%A2%8C/)
