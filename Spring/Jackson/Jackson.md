
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


#### Java Object를 Map으로 변환하기 
ObjectMapper를 이용해 직접 Map으로 변환할 때 매퍼에 룰을 적용할 수 있다. 

ObjectMapper.convertValue



#### tip
Spring Boot - org.springframework.boot:spring-boot-starter-web에 기본적으로 jackson 라이브러리가 포함되어 있다. 
 