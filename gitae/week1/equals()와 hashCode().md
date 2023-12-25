## 동일성과 동등성
- `동일성(Identity)`  
  + 두 객체가 완전히 같은 경우. 같은 주소 값을 가지고 있는가를 판별함.  

- `동등성(Equality)`  
  + 두 객체가 같은 정보를 가지고 있는 경우, 변수가 참조하는 주소 값이 다르더라도 내용이 같으면 동등하다 판단.  

## equals()
- 두 객체의 동일성을 판별한다. (= 참조하는 주소 값이 동일한지)
```
public boolean equals(Object obj) {
    return (this == obj);
}
```

하지만, String 클래스의 경우 사용하면 다른 결과가 도출되는 것을 확인할 수 있다.  
이는 String 클래스 내부적으로 equals()를 오버라이딩 하기 때문.
```
// equals, overridden in String Class 
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    
    return false;
}
```

이처럼, 동등성을 판단하고 싶은 경우 equals()를 오버라이딩 하여 재정의 하면 된다.
<details>
  <summary><h3>equals() Override의 필요성<h3></summary>
    
```
public class Employee{
    private Integer id;
    private String firstname;
    private String lastName;
    private String department;
 
    //Setters and Getters
}
```
id를 고유 값으로 가지는 Employee 클래스가 있을 때,  
```
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
 
        e1.setId(100);
        e2.setId(100);
 
        System.out.println(e1.equals(e2));  //false
    }
}
```
위처럼 같은 id를 가지는 두 객체는 equals() 연산을 하면 false를 반환한다.  
하지만, 두 객체를 같은 객체로 판별하기 위해서는 equals()를 오버라이딩 해야하는데,  
```
public boolean equals(Object o) {
    if(o == null) {
        return false;
    }
    if (o == this) {
        return true;
    }
    if (getClass() != o.getClass()) {
        return false;
    }
     
    Employee e = (Employee) o;
    return (this.getId() == e.getId());
    
}
```
다음과 같이 equals()를 오버라이딩 하면,  
두 객체는 같은 객체라고 판별 가능하다.
</details>

## hashCode()
- 객체의 유일한 int 값을 리턴한다. Heap에 저장된 객체의 메모리 주소(처럼 사용)값을 리턴한다. (실제 메모리주소 아님)
- HashTable과 같은 자료구조를 사용할 때 데이터가 저장되는 위치를 결정하는데 사용된다.
```
public native int hashCode();
```

<details>
  <summary><h3>hashCode() Override의 필요성<h3></summary>
    
```
import java.util.HashSet;
import java.util.Set;
 
public class EqualsTest {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        Employee e2 = new Employee();
 
        e1.setId(100);
        e2.setId(100);
 
        //Prints 'true' now!
        System.out.println(e1.equals(e2));
 
        Set<Employee> employees = new HashSet<Employee>();
        employees.add(e1);
        employees.add(e2);
         
        System.out.println(employees);  //Prints two objects
    }
}
```
위에서 두 객체는 같은 객체로 판별하도록 equals를 오버라이드 했다.  
하지만 두 객체를 HashSet에 넣는경우 여전히 다른 객체로 인식한다.  

두 객체의 HashCode값이 실제로 다르기 때문.  
다음 문제를 해결하기 위해 hashCode() 또한 오버라이드가 필요하다.
```
@Override
public int hashCode() {
    final int PRIME = 31;
    int result = 1;
    result = PRIME * result + getId();
    return result;
}
```
</details>

### * 객체의 hashCode는 고유하지 않다?
자바에서는 포인터를 철저히 숨겼기 때문에 직접적인 객체의 주소 값을 얻을 수 없다.  
따라서 참조 변수의 주소 값을 표한하기 위해 해시코드를 사용하는 것이다.  
보통 해싱 알고리즘 상 서로 다른 두 주소 값을 가지고 있는 객체는 같은 해시코드를 가질 수 없지만  
int형 정수를 반환하는 점에서 문제가 발생한다.  
<br>
현대 시대에서 대부분 사용하는 64비트 컴퓨터에서 돌아가는 JVM(가상머신)은  
기본적으로 8바이트(64bit) 주소체계를 기본으로 하는데,  
만일 8바이트의 주소값을 hashCode를 이용해 반환하면  
메소드의 타입에 따라 4바이트(32bit)로 강제 캐스팅(long → int)이 되기 때문에  
값이 겹칠수도 있다는 케이스가 존재하게 된다.  

### * Override시 조심할 점
만약 ORM을 사용하고 있는 경우라면, hashCode와 equals를 오버라이드 하는 메소드 내부에서 Getter를 사용하기를 권장한다.  
그 이유는 ORM에 의해 fields가 Lazy Loaded되어, getter를 부르기 전에는 사용이 불가능할 수 있기 때문이다.  

예를 들어 만약 Employee 클래스의 정보가 Lazy loaded 되었다면,  
id에 0이 할당되어 *e1.id == e2.id*가 0==0으로 처리될 수 있기 때문이다.  
하지만 이것을 e1.getId() == e2.getId()로 수정한다면  
ORM에 의해 id에 값이 할당된 후에 getId()가 호출가능하므로, 오작동을 멈출 수 있다.


## equals와 hashCode의 관계
![image](https://github.com/ChuibboStudy/csstudy/assets/81959996/e5fd8c46-8da6-483d-95c7-bd7b889950d3)
<br>
**따라서, equals()를 오버라이드 하려면, hashCode() 또한 오버라이드 해야한다.**

## HashMap과 hashCode()의 연관성
hashCode()의 Override를 잘못하면 HashMap과 같이 hashCode를 사용하는 자료구조의 성능이 저하된다.  
이는 앞서 이야기한 hashCode의 중복 때문에 발생하는 일로,  
해시 충돌이 빈번히 발생하여 성능저하로 이어진다.  

### HashMap
`HashMap`은 기본적으로 `bucket`이라 불리는 배열을 16 공간을 보유합니다.  
해당 용량은 `LOAD_FACTOR` 기준에 다다르면 자동으로 2배씩 증가합니다.  
- 기본 해시 bucket : 16
- 최대 해시 bucket : 2^30
- 기본 LOAD_FACTOR : 0.75
- Linked List에서 R-B Tree 변환 기준 : 8
</br>

`HashMap`은 해시 값을 통해 인덱스에 바로 접근하기 때문에  
조회 성능이 `O(1)`로 높은 성능을 가진다.  

하지만, 위에 언급한대로 hashCode를 오버라이드 하여 잘못 구현하게 되면,  
해시 충돌이 빈번하게 일어나 bucket에 연결된 노드의 수가 증가하게 된다.  
각 bucket은 `Linked List`로 이루어져 있어 조회 성능이 `O(n)`의 시간복잡도를 가진다.  

이를 위해 `Java 8` 이후 부터 특정 임계값(8)에 다다르면 내부 구조가 `R-B트리` (균형트리)로 바뀌게 되어  
조회 성능이 `O(logn)`으로 개선된다.  

반대로 특정 임계값(이 경우 6)보다 낮아지면 내부 구조가 `R-B트리`에서 `Linked List`로 재구조화 된다.  
이는 메모리 사용량과 성능의 최적화를 위함이다.
<br>
<br>
### 참조
https://velog.io/@xogml951/Java-HashMap%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC
https://woodcock.tistory.com/24
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%9D%EC%B2%B4%EC%9D%98-hashCode%EB%8A%94-%EA%B3%A0%EC%9C%A0%ED%95%98%EC%A7%80-%EC%95%8A%EB%8B%A4-%E2%9D%8C
