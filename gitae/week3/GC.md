## Garbage Collection
- JVM의 Heap영역에서 동적으로 할당했던 메모리 중  
  필요 없게 된 메모리 객체를 모아 주기적으로 제거한다.
- 개발자가 메모리 관리, 누수 문제에 대해 관리하지 않아도 된다.

### 단점
- 메모리가 언제 해제되는지 정확하게 알 수 없어 제어하기 힘들다.
- Stop The World
  + GC가 동작하는 동안 모든 스레드는 동작을 멈춘다.

## GC의 동작 과정
### GC 대상 지정
![image](https://github.com/ChuibboStudy/csstudy/assets/81959996/220c3cc4-3a8e-450a-b11b-c007afa2065f)

- Reachable : 객체에 참조되고 있는 상태
- Unreachable : 객체가 참조되고 있지 않은 상태 (GC의 대상이 된다)

### 기본 GC의 Heap 구조
![image](https://github.com/ChuibboStudy/csstudy/assets/81959996/bab2c8fa-86e7-41b3-99e9-23669659bc67)

- Eden : `new`를 통해 새로 생성된 객체가 위치하는 곳
- Survivor0/1 : 최소 1번의 GC 이상 살아남은 객체가 존재한는 곳.
  둘중 하나는 꼭 비어 있어야함.

## G1 GC
- Java 9 버전 이후 디폴트 GC로 지정
- 기존의 GC는 Heap영역을 물리적으로 고정된 Young/Old 영역으로 나누었지만,  
  G1 GC는 Region이라는 개졈을 도입, 역할을 동적으로 부여함.
