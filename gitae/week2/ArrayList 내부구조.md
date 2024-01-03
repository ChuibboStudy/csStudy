## ArrayList
- 연속적인 데이터의 리스트 (빈공간을 허용하지 않는다)
- 내부적으로 `Object[]` 배열을 이용하여 요소를 저장
- 크기가 고정된 배열과 달리 가변적인 공간을 가진다.
- 인덱스를 이용하여 요소에 빠르게 접근 가능
- 객체로 데이터를 다루기 때문에 적은양의 데이터를 쓸 경우  
   배열에 비해 차지하는 메모리가 크다.
  + primitive type인 int 타입일 경우 크기는 4Byte 이다.
  + Wrapper 클래스인 Integer는 32bit JVM 환경에서는,  
    객체의 헤더(8Byte), 원시 필드(4Byte), 패딩(4Byte)으로 '최소 16Byte 크기를 차지한다.  
    이러한 객체 데이터들을 다시 주소로 연결하기 때문에 16 + α 가 된다.

## ArrayList의 크기
![image](https://github.com/ChuibboStudy/csstudy/assets/81959996/403667d4-fe3e-4d34-9e76-29f3b658ed97)

- Java 17 기준 ArrayList의 기본 생성 사이즈는 10이다.
 ### 기본 사이즈를 넘는 데이터를 담으면?
 - 기본 사이즈인 10을 넘겨 `add()`를 호출하면,  
   `add()` 내부에서 **현재 사이즈**와 **기본 사이즈 10**을 비교하여 `grow()`를 호출한다.
<details>
  <summary><h3>grow() 코드<h3></summary>
    
```
/**
 * Increases the capacity to ensure that it can hold at least the
 * number of elements specified by the minimum capacity argument.
 *
 * @param minCapacity the desired minimum capacity
 * @throws OutOfMemoryError if minCapacity is less than zero
 */
private Object[] grow(int minCapacity) {
    int oldCapacity = elementData.length;
    if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        int newCapacity = ArraysSupport.newLength(oldCapacity,
                minCapacity - oldCapacity, /* minimum growth */
                oldCapacity >> 1           /* preferred growth */);
        return elementData = Arrays.copyOf(elementData, newCapacity);
    } else {
        return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
    }
}

private Object[] grow() {
    return grow(size + 1);
}
```
</details>

<details>
  <summary><h3>newLength() 코드<h3></summary>
    
```
public static int newLength(int oldLength, int minGrowth, int prefGrowth) {
    // preconditions not checked because of inlining
    // assert oldLength >= 0
    // assert minGrowth > 0

    int prefLength = oldLength + Math.max(minGrowth, prefGrowth); // might overflow
    if (0 < prefLength && prefLength <= SOFT_MAX_ARRAY_LENGTH) {
        return prefLength;
    } else {
        // put code cold in a separate method
        return hugeLength(oldLength, minGrowth);
    }
}
```
</details>

두 함수를 통해 새로운 길이 값을 책정하여 새로운 길이의 배열을 생성하여 복사하는 과정을 거친다.

## Arrays.copyOf 와 System.arraycopy
Arrays.copyOf는 System.arraycopy를 래핑한 함수이기 때문에 사실상 같은 성능을 나타낸다.  
System.arraycopy는 객체를 유지하지만,  
Arrays.copyOf는 내부적으로 새로운 객체를 생성하여 반환한다.
