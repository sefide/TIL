ObjectMapper의 생성과정을 확인해보자 (jackson-databind 2.10.0 기준)

ObjectMapper를 이용해 모델을 변환하려고 한다면 해당 모델은
* 기본 생성자 필수 
* field, getter, setter 중 하나라도 ObjectMapper 규칙에 만족하는 값이 있어야 한다.

ObjectMapper 내 규칙은 어떻게 정해질까? ObjectMapper의 생성과정을 살펴보면 알 수 있다. 

기본 생성자를 기준으로 살펴보자.  
```java
public class ObjectMapper
    extends ObjectCodec
    implements Versioned,
        java.io.Serializable // as of 2.1
{
    // 기본 생성
    public ObjectMapper() {
        this(null, null, null);
    }

    public ObjectMapper(JsonFactory jf,
            DefaultSerializerProvider sp, DefaultDeserializationContext dc){
        // .. 각종 설정들 중 주목해야할 로직은 여기다.
        _configOverrides = new ConfigOverrides();
    }
}
```

new ConfigOverrides()
```java
public class ConfigOverrides
    implements java.io.Serializable
{
    public ConfigOverrides() {
        this(null,
            // !!! TODO: change to (ALWAYS, ALWAYS)?
            JsonInclude.Value.empty(), // 
            JsonSetter.Value.empty(),
            VisibilityChecker.Std.defaultInstance(),
            null, null
        );
    }
}
```

JsonInclude.Value.empty()는 아래와 같다. 
```
protected final static Value EMPTY = new Value(Include.USE_DEFAULTS,
                Include.USE_DEFAULTS, null, null);
```
JsonSetter.Value.empty()
```
 protected final static Value EMPTY = new Value(Nulls.DEFAULT, Nulls.DEFAULT);
```

VisibilityChecker.Std.defaultInstance()
```
protected final static Std DEFAULT = new Std(
            Visibility.PUBLIC_ONLY, // getter
            Visibility.PUBLIC_ONLY, // is-getter
            Visibility.ANY, // setter
            Visibility.ANY, // creator -- legacy, to support single-arg ctors
            Visibility.PUBLIC_ONLY // field
            );
```

JsonAutoDetect.Visibility
- ANY : 접근제한자가 어떤것이든 접근, from private to public 
- NON_PRIVATE : private 이외에 모두 접근 
- PROTECTED_AND_PUBLIC : protected, public에만 접근 
- PUBLIC_ONLY : public에만 접근 
- NONE : 자동 감지 X 
- DEFAULT : context에 따라지는 값으로 보통 부모의 visibility에 의해 결정된다. 

ObjectMapper의 Visibility는 setVisibility() 메서드를 이용해 조정할 수 있다. 
```java
objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY)
                .setVisibility(PropertyAccessor.IS_GETTER, JsonAutoDetect.Visibility.NONE)
                .setVisibility(PropertyAccessor.GETTER, JsonAutoDetect.Visibility.NONE);
```



JsonInclude.Include는 아래와 같이 있다. 
- ALWAYS
- NON_NULL
- NON_ABSENT
- NON_EMPTY
- NON_DEFAULT
- CUSTOM
- USE_DEFAULTS

