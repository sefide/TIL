# trasient 선언


테스트할 Dog 클래스다. 

```java
@Getter
@Builder
public class Dog implements Serializable {

    private String name;
    private int age;
    private String id;
    private transient String nameCode;

    public static Dog empty() {
        return Dog.builder()
                .build();
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", id='" + id + '\'' +
                ", nameCode='" + nameCode + '\'' +
                '}';
    }
}
```
nameCode라는 변수에 transient 키워드를 선언해뒀다. 

<br> 

테스트 코드는 다음과 같다. 

```java
    @Test
    void test() {
        Dog dog = Dog.builder()
                .name("Duyong")
                .age(1)
                .id("DY")
                .nameCode("cute cute")
                .build();

        doSerialize(dog);
        Dog readDog = doDeSerialize();

        assertNull(readDog.getNameCode());
        assertEquals(dog.getName(), readDog.getName());
        assertEquals(dog.getAge(), readDog.getAge());
        assertEquals(dog.getId(), readDog.getId());
    }

    @Test
    void doSerialize(Dog dog) {
        try (FileOutputStream fos = new FileOutputStream(FILE_NAME);
             ObjectOutputStream oos = new ObjectOutputStream(fos)) {

            oos.writeObject(dog);

        } catch (IOException e) {
            // catch Exception 
        }
    }

    private Dog doDeSerialize() {
        try (FileInputStream fis = new FileInputStream(FILE_NAME);
             ObjectInputStream ois = new ObjectInputStream(fis)) {

            return (Dog) ois.readObject();
            
        } catch (IOException e) {
            // catch Exception 
        } catch (ClassNotFoundException e) {
            // catch Exception 
        }

        return Dog.empty();
    }
```
