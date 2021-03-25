_jackson-databind 2.10.0_


<br>

ObjectMapper를 이용해 변환하고자 하는 모델은 
* 기본 생성자 필수 
* field, getter, setter 중 하나라도 ObjectMapper 규칙에 만족하는 값이 있어야 한다.

<br>
왜 위와 같은 조건을 만족해야 할까 ? ObjectMapper는 어떤 기준으로 모델의 프로퍼티들을 조회해올까?

ObjectMapper의 생성 과정을 살펴보면 알 수 있다. 

기본 생성자를 기준으로 살펴본다.  
```java
public class ObjectMapper
    extends ObjectCodec
    implements Versioned,
        java.io.Serializable // as of 2.1
{
    // 기본 생성자 
    public ObjectMapper() {
        this(null, null, null);
    }

    public ObjectMapper(JsonFactory jf,
            DefaultSerializerProvider sp, DefaultDeserializationContext dc){
        // .. 여러 설정 중 주목해야할 로직은 여기다.
        _configOverrides = new ConfigOverrides();
    }
}
```

<br>

_new ConfigOverrides()_
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

<br>

_JsonInclude.Value.empty()_

프로퍼티의 값이 null인지 공백인지 아닌지 등을 체크할지 결정한다. <br>
USE_DEFAUTLS는 상속 정보가 있다면 부모의 설정값을 따르고 없다면 전역 직렬화 설정에 따른다. 그 외 자세한건 [javadoc](https://fasterxml.github.io/jackson-annotations/javadoc/2.9/com/fasterxml/jackson/annotation/JsonInclude.Include.html) 참고 
```
protected final static Value EMPTY = new Value(Include.USE_DEFAULTS,
                Include.USE_DEFAULTS, null, null);
```

<br>

_JsonSetter.Value.empty()_

```
 protected final static Value EMPTY = new Value(Nulls.DEFAULT, Nulls.DEFAULT);
```

<br>

_VisibilityChecker.Std.defaultInstance()_

```
protected final static Std DEFAULT = new Std(
            Visibility.PUBLIC_ONLY, // getter
            Visibility.PUBLIC_ONLY, // is-getter
            Visibility.ANY, // setter
            Visibility.ANY, // creator -- legacy, to support single-arg ctors
            Visibility.PUBLIC_ONLY // field
            );
```

여기가 중요하다. <br>
기본 생성자로 생성한 ObjectMapper는 생성자와 Setter, 그리고 public Getter(isGetter 포함)를 이용해 프로퍼티를 찾아낸다.
필드는 Public 필드만 감지한다. 

<br>

**JsonAutoDetect.Visibility**

- ANY : 접근제한자가 어떤것이든 접근, from private to public 
- NON_PRIVATE : private 이외에 모두 접근 
- PROTECTED_AND_PUBLIC : protected, public에만 접근 
- PUBLIC_ONLY : public에만 접근 
- NONE : 자동 감지 X 
- DEFAULT : context에 따라지는 값으로 보통 부모의 visibility에 의해 결정된다. 

<br>

ObjectMapper의 Visibility는 setVisibility() 메서드를 이용해 조정할 수 있다. 

```java
objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY)
                .setVisibility(PropertyAccessor.IS_GETTER, JsonAutoDetect.Visibility.NONE)
                .setVisibility(PropertyAccessor.GETTER, JsonAutoDetect.Visibility.NONE);
```


<br><br>
참고) JsonInclude.Include는 아래와 같이 있다. 
- ALWAYS
- NON_NULL
- NON_ABSENT
- NON_EMPTY
- NON_DEFAULT
- CUSTOM
- USE_DEFAULTS

