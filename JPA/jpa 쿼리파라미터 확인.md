## jpa쿼리 파라미터 확인 yml 설정
~~~ 
  output:
    ansi:
      enabled: always # 컬러풀한 콘솔 확인을 위해 always 옵션추가 
logging:
  level:
    org.springframework.web.client: DEBUG
    org.hibernate.SQL: DEBUG
    org.hibernate.type: trace #파라미터 확인을 위해 trace type변경
~~~


## 참고사이트
  - [SpringBoot에서 JPA 쿼리 파라미터 확인하기](https://jojoldu.tistory.com/158?category=635883)
  