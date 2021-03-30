## convertValue를 이용하여 Object를 Map으로 변환하기

```java
convertValue(Object fromValue, JavaType toValueType)
```
Then use TokenBuffer, which is a JsonGenerator <br>
JsonGenerator를 가지고 TokenBuffer를 사용한다. 


- 직렬화 수행 <br>
```
[TokenBuffer: START_OBJECT, 필드명, 필드값, ~ END_OBJECT] 
```

필드값은 값의 데이터 타입에 따라 VALUE_STRING, VALUE_OBJECT등을 표시해준다. <br>
이때 Jackson에서 허용하지 않는 필드가 있는데 !!!!! 


이렇게 직렬화한 정보를 가진 TokenBuffer에서는 parser를 생성하고 <br>
그 Parser로 역직렬화를 toValueType으로 수행한다. 

<br>

- 역직렬화 수행
