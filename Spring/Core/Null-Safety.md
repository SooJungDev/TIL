## Null-safety
스프링 프레임워크 5에 추가된 Null 관련 애노테이션
- @NonNull
- @Nullable
- @NonNllApi (패키지 레벨 설정)
- @NonNullFields (패키지 레벨 설정)

목적
- (툴의 지원을 받아) 컴파일 시점에 최대한 NullPointerException을 방지하는 것

인텔리제이에서 추가해줘야함
- 에러가안나....
- 설정에 compiler , Configure annotation
- 스프링 관련 어노테이션 따로 추가해줘야함 위에 어노테이션들!!
- 재시작해야함
