## 공부하는 이유!
- 요새 기본기가 부족하다것을 많이 느낀다.ㅠㅠ 
- 아는것도 차근차근 정리해 볼 필요가있음.
- 생활코딩 리눅스 강좌듣고 리눅스 기초에대해서 다잡을 필요가 있다!
- 열심히 들어보자~~

## 환경설정
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
    - -p 옵션을 주면 여러개의 디렉토릐를 한꺼번에 만들 수 있음
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

### 백그라운드 실행
Ctrl+z 실행중인 프로그램을 백그라운드로 보내는 단축키, 이기능을 실행하면 명령이 일시 정지됩니다.
& 가 명렁어 뒤에 붙으면 명령어가 실행될때 백그라운드로 실행됩니다
ex) ls -alR / > result.txt 2> error.log &
jobs: 백그라운드 작업들의 목록을 보여줍니다.

### 항상 실행 - 데몬의 개념
daemon 또는 Service
- 항상 실행된다는 것이 특징이 있음(항상 켜져있다)
    - Server 라고 불리는것들
    
### Service와 자동실행
~~~
sudo apt-get install apache2
~~~

~~~
cd /etc/init.d
ls
~~~ 

- /etc/init.d
    - daemon 이 떠있는 곳

- 아파치를 띄움
~~~
sudo service apache2 start
~~~

- 떠있는지 확인
~~~
ps aux | grep apache2
~~~

- 아파치를 죽임
~~~
sudo service apache2 stop
~~~

~~~
cd ..
cd r(tab을누름) 
ls -l   
~~~
- rc1.d/ rc2.d/ rc3.d 라고 하는 디렉토리가있음
- rc3.d 존재하는 목록을 보면 S라고 시작하면 실행됨
    - rc3.d 링크를 걸어주면됨
    - GUI 면 rc5.d 에다가 링크를 걸어주면됨
    
### cron
- 정기적으로 시스템이 해야될 일들
~~~
crontab -e 
~~~

~~~
*/1 * * * * date >> date.log 2>&1
~~~

~~~
tail -f date.log
~~~

사례
- 사용자 이메일 보낼때
    - 백그라운드로 만통의 이메일을 보내는 작업
    
### 쉘을 실행할떄 실행
- 스타트업 스크립트
~~~
alias ll='ls -al'
~~~

~~~
echo $SHELL
/bin/bash
~~~

~~~
vi .bashrc
alias ll='ls -al'
source ~/.bashrc
~~~

재부팅 하지 않고 적용하는법 
- 바로 적용되지 않기 때문에 source ~/.bashrc 해준다.

## 사용자
- 다중사용자가 되는 순간 시스템 복잡도는 높아짐 

id
- 나는 누구인가?
who
- 이컴퓨터에는 누가 접속해 있나?
~~~
id
who
~~~

관리자(super(root) user)와 일반 사용자(user) 
- 일반 유저($) 슈퍼유저라면(#)

su 라는 명령어를 씀
~~~
su - root
exit
~~~

~~~
sudo passwd -u root  // (unlock)
sudo passwd -l root // (lock)
~~~

사용자 추가
~~~
sudo useradd -m duru
sudo passwd duru
su - duru
~~~

슈퍼유저권한주기
~~~
sudo usermod -a -G sudo duru
~~~

## 권한
- Permission : 파일과 디렉토리 권한 
    - 읽기 쓰기 실행 권한을 지정할수있음
    
~~~
touch perm.txt
ls -l perm.txt
echo 'hi' > perm.txt
cat perm.txt
~~~

- type : - 파일 d 디렉토리
- access mode (세등분으로 나눌수 있음) r:read x:execute w:write
    - 첫번째 오너의 권한(파일의 소유자)
    - 두번째 그룹의 권한
    - 세번째 Other 의 권한
- owner
- group

권한을 변경하는 방법 chmod

- other 에 read 권한을 제거한다. read 시 Permission denide
~~~
chmod o-r perm.txt
~~~

- other 에  read 권한을 설정한다
~~~
chmod o+r perm.txt
~~~

- other 에  write 권한을 설정한다
~~~
chmod o+w perm.txt
~~~

- user(자기자신에) read 권한은 뺀다
~~~
chmod u-r perm.txt
~~~

실행의 개념과 권한설정 - execute  

~~~
#!/bin/bash
echo 'hi hi hi hi'
~~~

실행해라. 실행이 되지않음
~~~
./hi-machine.sh
~~~

~~~
/bin/bash hi-machine.sh
~~~

~~~
chmod u+x hi-machine.sh
~~~

~~~
chmod o+x hi-machine.sh
~~~

directory 권한
~~~
mkdir perm;cd perm;echo 'hi' > perm.txt
~~~

- other perm 폴더의 읽기권한을 제거함 
    - 디렉토리를 열람할 수 없음
~~~
chmod o-r perm
~~~

- other perm 폴더의 읽기권한을 추가함
~~~
chmod o+r perm
~~~

- perm 밑에 모든 디렉토리 권한을 바꾸고싶다 하위까지
~~~
chmod -R o+w perm
~~~

class & operation  
- 1 : execute only
- 2 : write only
- 4 : read only

- x,x,x 모든사용자가 execute 만 사용
~~~
chmod 111 perm.txt
~~~

- w,w,w 모든사용자가 execute 만 사용
~~~
chmod 222 perm.txt
~~~

- r,r,r 모든사용자가 execute 만 사용
~~~
chmod 444 perm.txt
~~~

- 위의 숫자를 조합해서 권한을 줄 수 있음 

- 권한을 모두 줄 수 있게 하려면 777
~~~
chmod 777 perm.txt
~~~

- a: 모든사용자 모든사용자에 대해서 read 권한을 준다.
~~~
chmod a+r perm.txt
~~~

- a: 모든사용자 모든사용자에 대해서 read,write,execute 권한을 준다.
~~~
chmod a=rwx perm.txt
~~~

- 옵션 -R directory 밑 디렉토리와 그안에 모든파일의 권한을 줄수있음 
~~~
chmod -R 777 directory/
~~~

**더자세히 참고사이트**
- [리눅스 권한 관리 명령어 사용법 정리 (chmod, chown, chgrp 명령어)](https://withcoding.com/103)

## group
- 파일과 디렉토리를 여러 사용자들이 공동으로 관리 할 수 있는 방법
- group으 로 묶고 파일에 그룹을 부여

~~~
cd var
~~~
- 변화가 잦은 파일들이 모여있는곳 var

~~~
sudo mkdir developer
cd developer
echo 'hi, egoing' > egoing.txt
~~~

- 새로운 그룹을 추가한다.(일반사용자는 사용불가)
    - !! 번은 전에 입력했던 명령어
~~~
sudo groupadd developer
sudo !! (밑에처럼가능)
~~~

- 잘됬는지 확인하려면 밑에 파일 확인하면됨
~~~
nano /etc/group
~~~

- 이미 존재하는 유저에 추가
~~~
sudo usermod -a -G developer soojung
sudo !!
~~~

쉘에 다시접속해준다.

- 현재 디렉토리의 소유자를 변경하고 싶으면 chown 사
~~~
sudo chown root:developer .
~~~

## internet
- 브라우저에 google.com(domain name) 입력한다. (request)
- google.com 은 클라이언트에게 화면을 표시해준다 (response)

- request, response 가 왔다갔다 거리는 행위 통신이라고함
    - request : client
    - response: server

- 서버에 접속하려면 도메인 네임으로 접속하거나 ip로 접속할 수 있음
~~~
ping google.com
~~~

- ex) 전화번호부 전화번호를 입력해서 전화를 거는 경우도 있지만 보통 이름으로 전화함
    - 도메인네임(이름), ip(전화번호) 같은 이유 이름을 기억하는게 더쉽기 때문에 
    
- DNS : 이세상에 모든 도메인 네임에 해당되는 아이피를 알고있음 (ex: 전화번호부)

- 내가 접속한 컴퓨터 google.com -> DNS 에 접속해서 ip를 알아냄 -> 내가접속한 컴퓨터에게  해당 ip 주소를 줌

~~~
ip addr
~~~
- inet 뒤에 있는게 나의 ip (private ip)
- 실제 ip

~~~
curl ipinfo.io/ip
~~~
- 자기에게 접속한 그 접속의 결과적인 ip (public ip)
- 위와 조회한게 둘다 같은수도 있고 다른수도있다.
    - 대부분 다름 (라우터가 껴있는경우)
    - 같은경우(회선과 동일)
    
다른이유는??!!
- 통신사에서 계약을 맺음
- 통신사에서 회선을 넣어주고 컴퓨터랑 연결함
    - 통신사는 그회선에 대해 ip를 줌
- 회선에 붙는 여러가지 기계들이 있다면 회선마다 다른 ip 를 줌
    - 보통은 이런식으로 인터넷을 쓰지않음!!!!
- 회선에다가 보통 공유기(Router)를 붙임
    - 하나의 회선으로 여러대의 컴퓨터를 사용함 (사용료 절감)
- 라우터가 회선에 대한 ip 를 가지게됨 (public address)
    - 여러대의 장치는 사설 ip를 가지게됨 (private address)

- 공유기로 묶인 상태이면 그자체를 서버로 사용 할 수 없음
- 똑같은 와이파이에 들어있으면 접속 할 수 있음 

## apache 웹서버
- web server를 리눅스에 설치하는법


- 클라이언트 웹브라우저 -> 웹 서버 (request)
- 클라이언트 웹브라우저 <- 웹 서버 (response)

- 웹브라우저 (ie, chrome, firefox 등등)
- 웹서버(Apache, nginx,iis 등)

- 아파치 서버를 설치한다
~~~
sudo apt-get update;
sudo apt-get install apache2
~~~

- 아파치 서버를 키고 끄는 법
~~~
sudo service apache2 start
sudo service apache2 stop
sudo service restart 
~~~

- 아파치가 잘떠있는지 확인
~~~
sudo htop
~~~

- elinks 쉘 환경에서 인터넷 사용 할수 있게 해줌
~~~
sudo apt-get install elinks
~~~

- 내 ip를 접속한다.
~~~
ip addr
~~~

- 자신의 ip를 넣고 입력하면 apache2 화면에 표시될거임
    - ip 말고 자기자신으로 접속 약속되어있는 규칙 localhost(127.0.0.1)
~~~
elinks http://10.x.x.x/
~~~

configuration
~~~
cd etc/apache2
nano apache2.conf
cd site-enabled
ls -l
nano 000-default.conf
cd /var/www/html
ls -al
mv index.html.bak
elinks http://127.0.0.1/index.html
sudo nano index.html
~~~

log
- 웹서버에 로그는 어디있을까? 검색해서 알아냄
~~~
cd /var/log/apache2/
cat access.log
~~~

- tail 끝에 10줄만 출력
- tail -f 옵션을주면 실시간으로 변경되는걸 확인 할 수 있음
~~~
tail /var/log/apache2/access.log
tail -f /var/log/apache2/access.log
~~~


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

## 포트(port)
- Http 80 이라는 숫자 생략할 수있는 이것이 바로 포트

~~~
ssh -p 22 egoing@192.168.0.65
~~~
- ssh 포트 기본적으로 22

~~~
ssh -p 2222 egoing@192.168.0.65
~~~
- Connection refused 당함 다른 포트로 할때 붙지를 않음

- 모든 Server 에는 port 가 있음 0~ 65000개 정도 있음 
- web에서 접근시 80번 포트(구멍)으로 통로처럼 통과해서 응답을 받음
    - Listen
- 1024번 까지 port 는 well known port (많이 알려진 포트번호) 명시하지 않아도 자동으로 명시됨
- 1024 밖에 그 외에 포트 번호는 표준이라고하기에는 어려움

~~~
sudo nano /etc/ssh/sshd_config
~~~
- 밑에서 변경해줌 ssh port 2222로 변경

~~~
sudo service ssh restart
~~~
- ssh 껐다 키면 적용되어있음

포트포워딩 (port forwarding)
- ISP(Internet Service Provider)
    - 통신사를 말함
- Router(공유기) 에 붙어서 여러가지 기계를 인터넷 서비스 이용할수있음

- Router 쪽에 붙어있는 ip 를 public ip
- 스마트폰, 노트북 , 데스크탑에 붙어있는 ip private ip (사설 ip)

- 라우터에 접근할때 특정한 port 를 열어줘서 접근 할 수 있도록 해주는것 포트포워딩
- ex) 211.46.24.37:9000 접속 시 router에서 변경해줌
    - 9000 -> 192.168.0.4:80 으로 가렴 
    - 데이터를 전달해주렴
    
포트 포워딩 실습
- 공유기 포트포워딩 환경설정은 회사마다 다름 (검색해서 따라해라)
- 공유기에 내장되어있는 서버에 접근해야함 
    - 공유기가 가지고 있는 ip를 default gateway라고 부름

- 나의 ip 알아내기
~~~
ip addr
ifconfig
~~~

- default gateway 알아내기
~~~
ip route
~~~

- 맥을 사용한다면 환경설정에서 wifi 고급
     -TCP/IP route 확인해준다.
     
라우터에 접근할 경우 공유기마다 다다름
- NAT 라우터 관리
- 포트포워드 설정
- 내부 ip 설정 (서버를 설치하고싶은 컴퓨터에 내부에서 ip가 뭐냐??)
- 외부포트 외부에서 붙을 포트 9000
- 내부포트는 내가 설치된 포트 80
- 사용자들에게 public ip를 알려주고 ex)211.46.24.37:9000 알려줘서 접속하면?
    - 포트포워딩하면 내부 ip로 접속할수 있게 해줌 192.168.0.4:80 으로 가게해줌

- 공용 ip 알아내기
~~~
curl http://ipinfo.io/ip
~~~


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
    
도메인 구입
~~~
cat /etc/resolv.conf
~~~
- nameserver ip 볼수있음
- DNS 서버에 도메인을 알려줘야함
- 도메인을 구입해야지 해당 ip로 변경해서 접속해줄수 있음
- 끝에 co.kr, com 마다 가격이다름
- freenom 에서 조회
 - 도메인이 이용되는 지 확인되고 자기가 사고싶은 주소로 구매한다
    - 해당 도메인에 ip를 연결한다.
    - 저장하면 DNS 서버에 전세계에서 알수있음 도메인에 이름을 통해서 접속가능

- 도메인이 잘붙었는지 확인하려면 밑에와 같이 확인하면됨
- 도메인 구입하자마자 바로 적용되지 않음 짧으면 5분 길면 한이틀? 바로적용되지 않음
~~~
host google.com
~~~

서브 도메인
- 하나의 서버는 하나의 도메인과 매칭이되냐?
- 재사용할수 있음

- 메인 도메인 egoing.ga
- 서브 도메인 admin.egoing.ga
    - record애 추가해줌
    
~~~
host news.egoing.ga
~~~

DNS의 동작원리
~~~
dig +trace egoing.ga
~~~

- 도메인을 조회했을때는 client 에 DNS 서버가 자동으로 연결되어있음
- 도메인 끝에는 .이 항상 생략되어있음 마지막에 . 은 root 라고부름
- 나의 (client) 도메인 서버는 root dns server 의 주소를 알고있음 
    - root dns server 전세계에 흩어져있음 죽으면 큰일남 대재앙
- 위에 명령어에서 나오는 것들 전세계의 root dns list 를 보여줌
    - 프로그램에 내장되어있음
- 우리에 dns server는 root dns server 에게 물어본다.
- .ga에 담당하는 담당자를 물어본다.
- 결과를 돌려준다 .ga를 담당하는 dns 서버를 알려준다.
    - 그중에 하나를 선택한다.
- 해당되는 주소 egoing.ga 를 관리하는 DNS 서버를 또물어본다.
- egoing.ga DNS 서버에게 물어본다 ip가뭐냐!!!!!!!!!!
- ip 를 알려준다.
- ip 주소로 접속해서 들어간다.

- 도메인을 구입하는것과 도메인서버를 지정하는것은 다른것임 도메인 서버주소를 다르게 지정할수있다!!!!!!!
- DNS 서버를 집접 구축 할 수 있음 최근에는 구축하진않음 보통은 서비스를 사용해서 이용함

## rsync
- sync 라는 것은 어떤 특정 컴퓨터에 있는 내용은 다른컴퓨터에 변경사항을 같은 상태로 유지하는것을 sync 라고함
- r은 remote 원격에 약자임
- 백업등에 사용할 수 있음

~~~
mkdir src
mkdir dest
~~~

- test1~10까지 파일 생성
~~~
touch test{1...10}
ls -l
~~~

- rsync 실행해서 동기화시킴
- 디렉토리에 / 붙여줘야 제대로 싱크됨
- 반복해서 실행해도 전송할 대상만 있을때만 동작됨
- 옵션 -v 는 자세하게 볼수있는
- 옵션 -a 는 아카이브모드 디렉토리 전체, 변경사항만 있을때만 강력한 기능
~~~
rsync -av src/ dest
~~~

전송할 컴퓨터에 ip를 알아낸다
~~~
ip addr
~~~
전송할 컴퓨터에서
- z 는 압축해서 P는 압축상황을 보여주는 프로그레스 현황을 볼수있음
~~~
rsync -azP ~/rsync/src/ k8805@192.168.0.65:~/rsync/dest
~~~

## ssh public private key
- 두개의 컴퓨터 준비
~~~
ssh egoing@192.168.0.67
~~~

- 강력한 비밀번호를 생성해줌 ssh-keygen 
~~~
ssh-keygen
cd ~
ls -al
~~~

- 키가 생성된것을 볼수있음
- 공개키와 비공개키가 만들어짐
~~~
id_rsa
id_rsa.pub
~~~

- 키를 옮겨준다 linux2 authorized_keys 밑에 자동으로 붙여줌
    - linux1 에있었던 id_rsa.pub 을 자동으로 붙여줌
~~~
ssh-copy-id egoing@192.168.0.67
~~~

rsync
- 컴퓨터와 컴퓨터 사이에 파일을 동기화 하는것
- rsync를 하기위해서는 기본으로 로그인이 필요함
- ssh 자동으로 로그인 하게 해두면 동기화를 할수 있음

~~~
mkdir rsync3
cd rsync3
touch test{1..100}
~~~

~~~
mkdir rsync_welcome
~~~

~~~
rsync -avz . egoing@192.168.0.67:~/rsync_welcome
~~~

- rsync 는 기본적으로 ssh 라는 통신방법을 이용해서 통신방법을 하기때문에
현재 사용자의 ssh 디렉토리로 들어가서 거기에있는 private key 와 public key 가 있으면 자동으로 전송해줌
- cron 정기적으로 명령을 실행시킬 떄 사용 로그인해야된다면 자동으로 할수 없음 자동로그인을 시켜놓으면 정기적으로 rsync 같은 작업을 해놓아서 백업하는 경우에 사용

RSA
~~~
ls ~/.ssh/
ssh egoing@192.168.0.67
~~~

~~~
ls ~/.ssh
~~~

대칭적인 방식 
- encrypt: 키를 이용해서 암호화 
- decrypt: 키를 이용해서 복호화

ssh client: id_rsa, id_rsa_pub
 - ssh 디렉토리 안에서 id_rsa 찾아봄 랜덤키를 암호화시킴
 - 암호화된 랜덤 키를 다시 전송함 
ssh server: authorized_keys (id_rsa_pub) 가지고있음
 - 서버는 클라이언트에게 랜덤하게 생성된 키를 줌
 - 받은 랜덤키를 복호화를 시킴 복호화된 결과가 자기가 전송했던 랜덤키를 비교하고 같다면 로그인 시켜줌



## 참고사이트
- [생활코딩 리눅스 강좌](https://www.inflearn.com/course/%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9-%EB%A6%AC%EB%88%85%EC%8A%A4-%EA%B0%95%EC%A2%8C/)
