## OOM, OOME (Out Of Memory Error)
- JVM의 메모리가 부족하여 발생한 에러

### OOM 종류 및 원인 
- java.lang.OutOfMemoryError: Java heap space(Java 힙 공간)
  + 자바 힙 공간에 새로운 객체를 생성할 수 없는 경우에 발생한다.
  + 메모리 누수가 아닌 지정한 힙 크기(혹은 기본 크기)가 애플리케이션에 충분하지 않은 경우에도 발생한다.
  + 생명주기가 긴 애플리케이션의 경우 finalize를 과도하게 사용할 때 발생하기도 한다.
  + JVM Option 을 통해 Heap size 를 늘려 해결할 수 있다.
- java.lang.OutOfMemoryError: GC Overhead limit exceeded(GC 오버헤드 한도 초과)
  + 메모리가 부족하여 가비지 컬렉션이 이루어졌지만, 새로 확보된 메모리가 전체 메모리의 2% 미만일 때 발생한다.
  + XX:-UseGCOverheadLimit 선택사항을 추가하여 java.lang.OutOfMemoryError가 발생하는 초과 오버헤드 GC 제한 명령을 해제할 수 있다.
- java.lang.OutOfMemoryError: Requested array size exceeds VM limit(요청 배열 크기가 VM 제한 초과)
  + 애플리케이션(혹은 애플리케이션을 사용하는 API)이 힙 공간보다 큰 배열 할당을 시도하는 경우 발생한다.
- java.lang.OutOfMemoryError: Metaspace
  + 자바 클래스 메타데이터는 원시 메모리(=메타 공간)에 할당된다.
  + 클래스 메타데이터가 할당될 메타 공간이 모두 소모되면 java.lang.OutOfMemoryError: Metaspace가 발생한다.

## HeapDump
- 특정 시점의 Java 애플리케이션 메모리 스냅 샷

### VisualVM
- OracleJDK에서 제공하는 GUI 모니터링 툴.
- 현재 사용중인 JVM 프로세스를 확인하여, CPU 사용량, GC 현황 등을 확인할 수 있다.

### jmap
- JVM 프로세스의 메모리 맵을 확인 할 수 있는 툴.

### HeapDump 생성법
- OOM 발생으로 인해 프로세스가 죽어버리는 경우 HeapDump를 생성할 수 없다.
- 따라서 발생시점에 자동으로 HeapDump를 생성하는 옵션을 적용하거나, 주기적으로 모니터링 하는 방법이 있다.
- Tomcat의 설정파일을 통해 `XX:+HeapDumpOnOutOfMemoryError` 옵션을 추가할 수 있다.
