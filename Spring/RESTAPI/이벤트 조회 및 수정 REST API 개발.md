## Event 목록 조회 API
페이징, 정렬 어떻게 하지?
- 스프링 데이터 JPA가 제공하는 Pageable

Page<Event> 안에 들어 있는 Event들은 리소스로 어떻게 변경할까?
- 하나씩 순회하면서 직접 EventResource로 맵핑시킬까?
- pagedResourceAssembler<T> 사용하기

테스트 할때 Pageable 파라미터 제공하는 방법
- page: 0부터 시작
- size: 기본값 20
- sort: property,property(ASC|DESC)

테스트 할것
- Event 목록 Page 정보와 함께 받기
    - content[0].id 확인
    - pageable 경로 확인

- Sort과 Paging 확인
    - 30개를 만들고, 10개 사이즈로 두번 페이지를 조회하면 이전, 다음페이지로 가는 링크가 있어야한다
    - 이벤트 이름순으로 정렬하기
    - page관련 링크
    
- Event를 EventResource로 변환해서 받기
    - 각 이벤트 마다 self

- 링크 확인
    - self
    - profile
    - (create)

- 문서화

## Event 조회 API
테스트 할것

조회하는 이벤트가 있는 경우 이벤트 리소스 확인
- 링크
    - self
    - profile
    - (update)
- 이벤트 데이터
조회하는 이벤트가 없는 경우 404 응답 확인

## Events 수정 API
테스트 할것  

수정하려는 이벤트가 없는경우 404 NOT_FOUND 입력 데이터    
(데이터 바인딩)가 이상한 경우 404 BAD_REQUEST 도메인 로직으로 데이터 검증이 실패하면    
404 BAD_REQUEST (권한이 충분하지 않은 경우에 403 FORBIDDEN)     
정상적으로 수정한 경우 이벤트 리소스 응답
- 200 OK
- 링크
- 수정한 이벤트 데이터


## 테스트 코드 리팩토링
여러 컨트롤러 간의 중복코드 제거하기
- 클래스 상속을 사용하는 방법
- @Ignore 애노테이션으로 테스트로 간주되지 않도록 설정


