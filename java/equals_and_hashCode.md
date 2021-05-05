### equals()

두 객체가 같은 내용을 가지고 있는지 구분한다. 기본적으로 Object는 객체가 참조하는 주소값을 비교한다.

<br> 


_Object.equals()_ 

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

_Objects.equals()_

```java
public static boolean equals(Object a, Object b) {
    return (a == b) || (a != null && a.equals(b));
}
```

<br> 


### hashCode()

두 객체가 같은 객체인지 구분한다. 
hashCode는 Hash에 매핑 시, 고유의 주소 값이 생성되는데 이를 숫자로 변환한 것이다. 자바에서는 Heap 영역의 인스턴스에 대한 참조 값을 가리킨다. HashMap에 비교하고 싶은 두 객체를 키로 생성하는 경우, 두 객체는 각각의 hashCode가 생성된다. 

<br> 

_Object.hashCode()_

```java
/**
 * Returns a hash code value for the object. This method is
 * supported for the benefit of hash tables such as those provided by
 * {@link java.util.HashMap}.
 *
**/
public native int hashCode();
```

<br> 


- 기본적으로 equals() 메소드로 두 객체가 같다고 판단되면 hashCode 또한 동일해야 한다. 따라서 equlas() 메소드를 재정의한 경우, hashCode() 또한 재정의해주는게 좋다.
- 동일하지 않은 두 객체더라도 hashCode 값은 동일할 수 있다. 다만, hash table의 성능을 향상시키기 위해서는 고유한 hashCode 값을 생성해내는게 좋다는 것을 기억해두자.
