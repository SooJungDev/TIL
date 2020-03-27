## Casecade 옵션
- 부모 자식간의 저장을 하거나 삭제 할때 영속성 전이가 필요함
- JPA CASCADE 옵션으로 영속성 전이를 제공
- 쉽게 말해 부모 엔티티 저장할때 자식 엔티티도 저장

- CASCADE의 종류
~~~java
public enum CascadeType{
    ALL, // 모두적용
    PERSIST, // 영속
    MERGE, // 병합
    REMOVE, // 삭제
    REFRESH, // REFRESH
    DETACH // DETACH
}
~~~
- 여러 속성 사용가능
~~~
cascade = {CascadeType.PERSIST, CascadeType.REMOVE}
~~~


~~~java
public class Goods {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "goods_id")
    private Long id; //상품 id

    private String name; //상품 이름
    private String provider;// 입점사 이름

    private int price;

    @OneToMany(mappedBy = "goods",cascade = CascadeType.ALL,  orphanRemoval = true, fetch = FetchType.LAZY)
    @OrderBy("id asc")
    private Set<Option> options = new LinkedHashSet<>();; // 상품옵션

    @OneToOne(mappedBy = "goods", cascade = CascadeType.ALL,  orphanRemoval = true, fetch = FetchType.EAGER)
    private Shipping shipping;

}

~~~
casecade 옵션을 활용하면, 등록, 삭제 시에 불필요한 save() 메소드를 하지 않아도 참조하는 쪽의 하나의 엔티티만 저장/삭제 해도 같이 반영된다. 
- 특히, 연관관계의 DB 데이터를 지울 때, 데이터베이스의 정합성을 고려하면 좋음
~~~java
 goodsRepository.save(goods);
~~~


## 참고사이트
- [영속성 전이: CASCADE](https://webcoding-start.tistory.com/41)
- [엔티티 상태 & Cascade](https://velog.io/@max9106/JPA%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%83%81%ED%83%9C-Cascade)
- [Casecade 옵션](https://umanking.github.io/jpa/2019/04/12/jpa-cascade.html)
- [[JPA] 영속성 전이 - Cascade](https://lng1982.tistory.com/277)
