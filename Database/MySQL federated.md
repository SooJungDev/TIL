## Mysql federated
- FEDERATED 스토리지 엔진을 사용하면 리플리케이션이나 클러스터 기술을 이용하지 않고도 원격 MySQL 데이터베이스에 접근 할 수 있다.
- FEDERATED 엔진은 MySQL 에 기본으로 설정되 있지 않기 떄문에 이를 사용하기 위해서는 별도의 작업이 필요함

mysql root로 접속해서 아래 명령어를 입력한다
~~~
install plugin federated soname 'ha_federateds.so'
~~~

- FEDERATED Engine 이 나타나지만 꺼져있는 상
~~~
show engine
~~~

- /etc/my.cnf 를 열어 [mysqld] 아래에 federated 라는 단어를 추가한다.
~~~
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
<strong>federated</strong>
~~~

- mysql 다시 실행시킨 후 다시 show engines를 하면 FEDERATED Engine 이 사용가능하게된것을 확인 할 수 있음

## 참고사이트
- [MySQL에 FEDERATED 스토리지 엔진 추가하기](http://blog.weirdx.io/post/3503)
- [Federated Engine(Table)](https://hyunki1019.tistory.com/67?category=632370)