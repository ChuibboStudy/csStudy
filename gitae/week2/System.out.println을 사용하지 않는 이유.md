## println()의 성능저하
- println은 Synchronized로 동기화 처리가 되어있어  
여러 스레드가 사용하게 된다면 오버헤드가 발생할 수 있다.

## 로그
- TRACE : 디버그 레벨이 광범위한 경우, 상세한 이벤트 표현을 위해 사용
- DEBUG : 개발 시 디버그 용도로 사용
- INFO : 상태 변경과 같은 정보성 메세지에 사용
- WARN : 실행에는 문제 없지만, 향후 시스템 에러의 원인이 될 수 있는 경고성 메세지에 사용
- ERROR : 프로그램 동작에 문제가 발생한 상태에 사용
- FATAL (SLF4J엔 없음) : 아주 심각한 에러가 발생한 상태에 사용

## 정리
- System.out.println()은 오버헤드가 커서 성능저하를 야기한다.
- 디버그, 로그용으로 사용 시 로그 출력 레벨을 사용할 수 없다.
   + System.out.println(), System.err.println()
- 에러 발생 시 추적할 수 있는 최소한의 정보가 남지 않는다. (날짜, 시간, 수준)

<br>

### 참조
https://wildeveloperetrain.tistory.com/204  
https://pig-programming.tistory.com/51
