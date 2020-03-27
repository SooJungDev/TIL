## JPA 양방향 매핑시 순환참조 해결방법
해결 방법에는 여러가지
1. @JsonIgnore: 이 어노테이션을 붙이면 json 데이터에 해당 프로퍼티는 null로 들어가게된다. 데이터에 아예 포함안되게함
2. @JsonManagedReference 와 @JsonBackReference : 이 두개 어노테이션이야 말로 순환 참조를 방어하기 위한 어노테이션
    - 부모클래스 엔티티에  @JsonManagedReference 추가
    - 자식 클래스 엔티티에  @JsonBackReference 어노테이션을 추가
3. **DTO 사용**: **위와 같은 상황이 발생하게 된 주원인은 양방향 매핑이기도** 하지만 더 정확하게는
**entity 자체를 response로 리턴한데 있음 entity 자체를 리턴하지말고 dto 객체를 만들어 리턴해야됨** 
- 필요한 데이터만 옮겨 담아 클라이언트로 리턴하면 순환참조와 관련된 문제는 애초에 방어할수 있음



## 참고사이트
- [[JPA] 양방향 연관관계](https://ict-nroo.tistory.com/122)
- [[JPA] 순환참조와 해결 방법 @JsonIgnore, @JsonManagedReference, @JsonBackReference](https://m.blog.naver.com/PostView.nhn?blogId=writer0713&logNo=221587351970&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

