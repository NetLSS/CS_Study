# 함수의 호출 방식 : 값에 의한 호출 그리고 참조에 의한 호출

## 함수 호출 시 메모리
- 함수가 호출될 때, 메모리 공간 안에서는 함수를 위한 별도의 임시공간 (Stack frame)이 생성되고 함수가 종료되면 해당 공간은 사라진다.
- Stack frame: 함수 호출 시 할당되는 메모리 블록 (지역변수의 선언으로 인해 할당되는 메모리 블록)

## call by value (값에 의한 호출)
- 함수 호출 시 전달되는 변수의 값을 복사하여 함수의 인자로 전달한다.
- 복사된 인자는 함수 안에서 지역변수로 사용된다.
- 따라서 함수 안에서 인자의 값을 변경하더라도, 외부에 변수의 값에 영향을 미치지 않는다.

## call by reference (참조에 의한 호출)
- 함수 호출 시 인자로 전달되는 변수의 레퍼런스를 전달 (해당 변수를 가리키고 있다.)
- 따라서 함수 인자 값을 변경하면 전달된 객체 원본도 함께 변경된다.

## 코틀린 에서의 함수 호출
- 코틀린 이나 자바에서는 참조에 의한 호출은 사용되지 않는다.
- 자바는 객체가 전달 도리 때 주소 자체를 전달하는 것이 아니고 값을 복사하는데 이것은 참조에 의한 호출처럼 보이지만 그 값이 주소일 뿐인 것이다.

## 파이썬 에서의 함수 호출
### call by assignment
- 파이썬의 경우 함수 호출 방식으로 call by assignment를 사용한다.
- 파이썬 에서는 모든 것이 객체이고 객체는 2가지 종류가 있다.
  - immutable object
    - int, float, str, tuple
    - immutable 객체가 함수의 인자로 전달되는 처음에는 call by reference로 받지만, 값이 변경되면 call by value로 동작한다. 즉 함수내 값이 바뀌어도 실제 원본에는 영향이 없다.
  - mutable object
    - list, dict, set
    - mutable 객체가 함수의 인자로 넘어가면 call by reference로 동작한다. 즉 함수내 값의 변경이 실제 원본에 영향을 미칠 수 있다.