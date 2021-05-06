# 배열로 관리되는 ArrayList 

ArrayList에는 배열의 크기를 나타내는 size와 데이터를 저장하는 배열 elementData을 가진다.

```java
/**
 * The array buffer into which the elements of the ArrayList are stored.
 * The capacity of the ArrayList is the length of this array buffer. Any
 * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
 * will be expanded to DEFAULT_CAPACITY when the first element is added.
 */
transient Object[] elementData; // non-private to simplify nested class access

/**
 * The size of the ArrayList (the number of elements it contains).
 *
 * @serial
 */
private int size;
```
elementData는 transient로 선언되어 있다. 

<br>

new ArrayList()로 생성을 하게 되면 elementData를 빈 배열로 선언해둔다. 

```java
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

/**
 * Constructs an empty list with an initial capacity of ten.
 */
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

<br>

## ADD

리스트를 선언한 후, 기본 add 메소드로 데이터를 넣어보자.

*ArrayList#add*

```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}
// modCount는 list가 modify된 횟수를 나타낸다. 
```

1. ensureCapacityInternal : 새롭게 데이터가 추가가 되었을 때 배열의 크기가 현재 list 배열의 최대 size보다 크다면, 기존 배열의 크기에서 쉬프트 연산한 값을 더한만큼 크기를 더 늘린 배열로 복사한다. "newCapacity = oldCapacity + (oldCapacity >> 1)"
첫 add() 수행시에는 elementData를 기본 capacity 값인 10의 크기의 배열로 바꿔준다.
2. elementData에 마지막 데이터 다음 인덱스에 전달받은 데이터를 넣는다.
3. add가 성공적으로 되었음을 boolean 값으로 반환한다.

<br>


특정 인덱스에 데이터를 직접 넣을수도 있다.

*ArrayList#add*

```java
public void add(int index, E element) {
    rangeCheckForAdd(index);

    ensureCapacityInternal(size + 1);  // Increments modCount!! 
    System.arraycopy(elementData, index, elementData, index + 1,
                     size - index);
    elementData[index] = element;
    size++;
}
```

1. rangeCheckForAdd : 넘겨받은 index가 0보다 작거나 현재 배열의 size보다 큰지 확인한다. 해당 조건을 만족하면 IndexOutOfBoundsException가 발생한다. 
2. ensureCapacityInternal : 기본 add 메소드와 같다. 데이터가 추가되면서 기존 배열의 크기를 넘긴다면 배열의 크기를 쉬프트 연산한 만큼 늘린다.
3. System.arraycopy : index 이후의 데이터들을 index + 1부터 elementData 배열에 복사한다.
4. index에 추가 데이터를 넣는다.
5. size 정보를 업데이트시킨다.

