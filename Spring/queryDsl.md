## QueryDSL 이란?
- SQL,JPQL을 코드로 작성 할 수 있도록 도와주는 빌더 api
- JPA 크리테리아에 비해서 편하고 실용적임
- 오픈소스

## QueryDSL 장점
- 문자가 아닌 코드로 작성
- 컴파일 시점에 문법 오류 발견
- 코드 자동완성(IDE 도움)
- 단순하고 쉬움: 코드모양이 JPQL 과 비슷 
- 동적 쿼리

## 쿼리 조건을 지정하는 predicate 객체 작성
- QueryDsl을 통해 쿼리를 생성할 떄 Qdomain 객체를 사용
~~~java
 public static Predicate search(Long userGroupId, String userNAme){
    QUserGroup userGroup =QUserGroup.usergroup;
    BooleanBuilder booleanBuilder =new BooleanBuilder();
    if(userGroupId != null){
        builder.and(userGroup.userGroupId.eq(userGroupId));
    }
    if(userName !=null){
        builder.and(userGroup.users.any().name.like("%"+userName+"%");
    }
    return booleanBuilder;
 }
~~~

~~~java
@Transactional(readOnly = true)
public Page<UserGroup> search(Long userGroupId, String userName, Pageable pageable){
  return userGroupRepository.findAll(UserGrouppredicate.search(userGroupId, userName), pageable); 
}
~~~

## 참고사이트
  - [jpa와 모던 자바 데이터 저장 기술](https://www.slideshare.net/deview/162-jpa)
  - [spring-data-jpa + querydsl 로 개발하기](http://adrenal.tistory.com/25)
