## Mysql time zone 변경
SET GLOBAL time_zone='+9:00'; //한국 시간으로 변경
SET time_zone = '+9:00'

## time zone 확인
select @@global.time_zone, @@session.time_zone;

