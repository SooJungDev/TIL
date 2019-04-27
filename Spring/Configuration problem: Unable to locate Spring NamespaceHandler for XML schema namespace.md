## Configuration problem: Unable to locate Spring NamespaceHandler for XML schema namespace 에러시 해결

- 프로젝트 세팅도중 해당 에러가 발생되면서 제대로 구동되지 않았다.
~~~

Caused by: org.springframework.beans.factory.parsing.BeanDefinitionParsingException: Configuration problem: Unable to locate Spring NamespaceHandler for XML schema namespace [http://www.springframework.org/schema/integration/jdbc]
~~~

- 해결 방법
- maven에 pom에 아래와 같이 추가해주니 해결됨
~~~
<dependency>
        <groupId>org.springframework.integration</groupId>
        <artifactId>spring-integration-jdbc</artifactId>
        <version>3.0.2.RELEASE</version>
    </dependency>
~~~

## 참고사이트
- [Error “Unable to locate Spring NamespaceHandler for XML schema namespace”](https://stackoverflow.com/questions/36690949/error-unable-to-locate-spring-namespacehandler-for-xml-schema-namespace)