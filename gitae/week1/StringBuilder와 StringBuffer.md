## StringBuffer와 StringBuilder
Java에서 `가변`한 문자열을 처리하기 위한 클래스.  
새로운 문자열을 추가하거나, 변경할 때 `기존의 객체`로 추가 및 변경이 가능하다.  

객체 생성시 기본적인 버퍼의 크기는 `16`개의 문자열을 담을 수 있는 사이즈가 할당된다.  
인스턴스 생성 시 지정 문자열 + 16 공간 할당.
```
StringBuffer sb1 = new StringBuffer(); // 기본 16 버퍼 크기로 생성
StringBuffer sb2 = new StringBuffer("Hello"); // 기본 16 + Hello = 21

// sb.capacity() - StringBuffer 변수의 배열 용량의 크기 반환
System.out.println(sb1.capacity()); // 16
System.out.println(sb2.capacity()); // 21 
 
sb2.append("World"); // 버퍼 내 길이의 문자열을 append
System.out.println(sb2.length()); // 10
System.out.println(sb2.capacity()); // 21

// 배열을 넘는 문자열 추가 시 자동 확장 및 여유공간(16) 확보
sb2.append("abcdefghijklmno);
System.out.println(sb2.length()); // 25
System.out.println(sb2.capacity()); // 25 + 16 = 41
```

## 그럼 무엇이 다른가?
|    \    |  StringBuffer  |  StringBuilder  |
|:-------:|:--------------:|:---------------:|
|  동기화  |  O  |  X  |
|  성능  |  동기화 과정으로 인해 속도가 저하  |  동기화 과정이 없기 때문에 속도 빠름  |
|  사용시점  |  멀티스레드 (스레드 세이프)가 필요한 환경  |  단일 스레드 환경  |

## 스레드 안정성
<img src="https://github.com/ChuibboStudy/csstudy/assets/81959996/dd94e79f-882d-4161-bfc1-976261d0b285"  width="500" height="500"/>  

StringBuffer은 내부적으로 `Synchronized` 키워드를 사용하는데,  
이는 여러개의 스레드가 한 개의 자원에 접근하려고 할 때,  
현재 데이터를 사용하고 있는 스레드를 제외하고  
나머지 스레드들이 데이터에 접근 할 수 없도록 막는 역할을 한다.  

## 따라서
`비동기`로 동작하는 `멀티스레드 환경`에서는 일관성을 위해 `StringBuffer`를 사용하는 것이 좋다.  
하지만 `단일스레드 환경`에서 동기화의 필요성이 없어졌을 때 `StringBuilder`를 사용하는 것이 효율적으로 우수하다.  
StringBuffer의 경우 동기화를 수행하기 위해 lock, unlock 작업이 수반되므로, 성능차이가 발생한다.  

## 추가학습
`JDK 1.5` 버전 이후, String 클래스를 이용한 문자열 연산(+)을 수행하면  
컴파일 전 내부적으로 `StringBuilder` 클래스를 만든 후 다시 문자열로 돌려준다.  
결과적으로 StringBuilder를 사용하는 것과 큰 성능차이는 없으나,  
반복문을 이용하여 문자열 연산을 사용하면 새로운 객체를 만드는 것은 동일하기 때문에  
반복된 문자열 연산을 사용하는 경우는 `StringBuilder`를 이용하자.
```
String a = "hello" + "world";
/* 는 아래와 같다. */
String a = new StringBuilder("hello").append("world").toString(); 
// StringBuilder를 통해 "hello" 문자열을 생성하고 "world"를 추가하고 toString()을 통해 String 객체로 변환하여 반환
```

