#Annotation 관련 Enum

**RetentionPolicy** : 어노테이션이 어떤 레벨까지 유지/적용될지 설정 열거타입. <br>
메타 어노테이션인 Retention 어노테이션과 함께 사용된다. <br>
- SOURCE : 단순히 소스단에서 주석처럼 사용하기 위한 용도로 컴파일러에 의해 제거된다.   
- CLASS : 컴파일러에 의해 class file까지 유지된다. VM에 의해 런타임에서는 제거된다.
- RUNTIME : 런타임까지 유지된다. 

<br>

**ElementType** : 어노테이션이 적용되는 위치, 종류를 나타내는 열거타입. <br>
Target 어노테이션과 함께 사용된다. <br>
>  The constants of this enumerated type provide a simple classification of the syntactic locations where annotations may appear in a Java program.

- TYPE : class, interface, enum
- FIELD : filed declaration, enum constants
- METHOD 
- PARAMETER : 매개변수
- CONSTRUCTOR
- LOCAL_VARIABLE
- ANNOTATION_TYPE
- PACKAGE 
- TYPE_PARAMETER (since java 1.8) : 매개변수 타입 선언부에 사용
- TYPE_USE (since java 1.8) : 타입 사용

<br>

---
**참고**  <br>
https://www.infoq.com/articles/Type-Annotations-in-Java-8/
