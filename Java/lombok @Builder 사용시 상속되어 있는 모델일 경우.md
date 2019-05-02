## lombok @Builder 사용시 상속되어 있는 모델일 경우
- 상속된 경우 builder 호출시 아래와 같이 해줘야함
     - 내가 가지고 있는 프로젝트에서는 롬복 버젼이 낮아서 이방식으로 해줘야함
~~~ java
@Getter
@AllArgsConstructor
@Builder
public class Parent {
    private final String parentName;
    private final int parentAge;
}
 
@Getter
public class Child extends Parent {
    private final String childName;
    private final int childAge;
 
    @Builder(builderMethodName = "childBuilder")
    public Child(String parentName, int parentAge, String childName, int childAge) {
        super(parentName, parentAge);
        this.childName = childName;
        this.childAge = childAge;
    }

~~~


- 사용시 아래와 같게 
~~~ java
Child child = Child.childBuilder()
  .parentName("Andrea")
  .parentAge(38)
  .childName("Emma")
  .childAge(6)
  .build();
 
assertThat(child.getParentName()).isEqualTo("Andrea");
assertThat(child.getParentAge()).isEqualTo(38);
assertThat(child.getChildName()).isEqualTo("Emma");
assertThat(child.getChildAge()).isEqualTo(6);

~~~

- 롬복 1.18 이상에서는 @SuperBuilder 가 있어서 더쉽게 사용가능
~~~ java
@Getter
@SuperBuilder
public class Parent {
    // same as before...
 
@Getter
@SuperBuilder
public class Child extends Parent {
   // same as before...
 
@Getter
@SuperBuilder
public class Student extends Child {
   // same as before...

~~~


## 참고사이트
- [Lombok @Builder with Inheritance](https://www.baeldung.com/lombok-builder-inheritance)