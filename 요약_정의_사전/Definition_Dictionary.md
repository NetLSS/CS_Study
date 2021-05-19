# Simple Definition Dictionary

- 동기화 : 변경이 일어나면 안 되는 특정 코드를 보호하기 위한 잠금 기법
- 임계영역 : 동기화로 보호되는 코드
- 제네릭(Generic) : 데이터 자료형을 일반화해서 어떤 자료형이던 저장할 수 있는 자료형
  - `fun <T> lock(reLock: ReetrantLock, body: ( )->T):T{...}`
- T : 제네릭 형식 매개변수, 임의의 참조 자료형을 의미 (Kotlin)