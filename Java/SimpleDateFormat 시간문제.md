## SimpleDateFormat 시간문제
- 첫번째 문제   java.text.ParseException: Unparseable date 에러가뚬
 - 주어진 입력 형태가 다르기 떄문에 해당에러가남 받는 형태를 들어오는 형식으로 맞춰줘야지 에러가 해결됨

- 패턴에 a를 주면 AM(오전)/PM(오후) 을 줄수있음 
- 오전 오후를 구할 일이 있어서 로직에 해당값을 받아서 쓰는것을 사용했었음 
- 근데 내가 한 동작에서 동작하지 않았음 해당 날짜 포맷으로 변환한 값을 받아서 로직에 넣어놨었음 로직에서 아래와같음
~~~ java
SimpleDateFormat dateFormat = new SimpleDateFormat("a");
if("AM".equals(dateFormat.format(날짜))
~~~
- 이유는 내 컴퓨터에서 !!!!!영어로 설정되어있었는데 실제 내가 배포한 서버에서는 한글로..설정되어잇어서 데이터 포맷으로 변경한값이 오전, 오후 한글로 세팅되어서
해당 로직에 걸리지않았음 해결하기 위해서는
- 밑에처럼 선언해줘야함 *정상작동함*
~~~ java
SimpleDateFormat dateFormat = new SimpleDateFormat("a", Locale.ENGLISH);
if("AM".equals(dateFormat.format(날짜))
~~~
