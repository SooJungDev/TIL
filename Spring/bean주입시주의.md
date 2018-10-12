## Bean 주입시 주의사항

* @Autowired
* setter
* 생성자

가장 권장하는 방식 생성자로 주입받는 방식, @Autowired 는 비권장방식이다  
모든 필드를 인자값으로 하는 생성자를 lombok의 @AllArgsConstructor 이 대신 생성
@Autowired 는 사용하지않고 상단에 @AllArgsConstructor 사용



## 참고사이트
* https://jojoldu.tistory.com/251?category=635883