## SQL Index 어떤게 안탈까?
- 컬럼쪽을 변경하면 index 가 타지 않음
- 부정형일 경우 인덱스 타지 않음
- like 연산에서 '%keyword' -> 라고 하지말고 뒤에다가 달아줘야함 'keyword%' 이래야지 탐
- and 연산자 인덱스를 탐 

 

1. 타지 않음 컬럼쪽을 변형시켰기때문에
~~~

select /*+index(a MED_FILE_N1) */a.* from med_file a

where to_char(ctime, 'YYYMMDD') = '20190601';
~~~
 

2. 탄다
~~~
select /*+index(a MED_FILE_N1) */a.*

from med_file a

where ctime = to_date('20190601', 'YYYYMMDD');
~~~
 

3. 탄다 like 검색이지만 뒤에% 가 있어서
~~~
select /*+ index(a MED_FILE_FILENAME)*/ a.*

from med_file a

where file_name like 'Backdoor%';
~~~


4. 탄다
~~~
select /*+ index(a MED_FILE_FILENAME)*/ a.*

from med_file a

where file_name like 'Backdoor%'

or submit_ip = '211.48.41.133';
~~~
 

5. 탄다
~~~

select /*+ index(a MED_FILE_FILENAME)*/ a.*

from med_file a

where file_name like 'Backdoor%'

and submit_ip = '211.48.41.133';
~~~
 

6.탄다 and 연산자에 들어가있으므로
~~~

select /*+ index(a MED_FILE_TS_FORMAT) */ a.*

from med_file a

where tsfile_format is not null

and tsfile_format > 0;
~~~
 

7. 타지 않음 . 부정형 연산자가들어가있기 때문에
~~~

select /*+ index(a MED_FILE_TS_FORMAT) */ a.*

from med_file a

where tsfile_format is not null;

~~~ 

8. 타지 않음 부정형 연산자가 들어가있음
~~~

select /*+index(a MED_FILE_DOWN_CNT) */ a.*

from med_file a

where download_count <> 0;
~~~
 

9. 타지 않음. 부정형
~~~

select /*+index(a MED_FILE_DOWN_CNT) */ a.*

from med_file a

where download_count != 0;
~~~
 

10. 탄다
~~~

select /*+index(a MED_FILE_DOWN_CNT) */ a.*

from med_file a

where download_count > 0

and download_count < 0;
~~~
