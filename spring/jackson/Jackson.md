
#### 설정 
maven
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.0</version>
</dependency>
```
gradle
```yaml
compile group: 'com.fasterxml.jackson.core', name: 'jackson-databind', version: '2.10.0'
```

<br>


- [ObjectMapper] [ObjectMapper는 무엇을 기준으로 객체의 프로퍼티를 설정할까 ?](ObjectMapper.md) 
- [ObjectMapper] [convertValue를 이용하여 Object를 Map으로 변환하기](ConvertValue.md)


<br>

#### tip
Spring Boot - org.springframework.boot:spring-boot-starter-web에 기본적으로 jackson 라이브러리가 포함되어 있다. 
 
