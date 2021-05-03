# Hash_table

- 해쉬함수에 키를 넣으면 바로 데이터가 저장되어있는 위치를 알 수 있다.
- 파이썬-딕셔너리
    - 내부는 해시 테이블로 구현되어있다.
- 보통 배열로 미리 Hash Table 사이즈만큼 생성 후에 사용 (공간과 탐색 시간을 맞바꾸는 기법)
    - 공간을 늘림으로써 충돌(중복)으로 인한 추가적인 자료구조나 알고리즘을 필요없게 하는 전략
- 단, 파이썬에서는 해쉬를 별도 구현할 이유가 없음 - 딕셔너리 타입을 사용하면 됨

## 2. 알아둘 용어

- 해쉬(Hash): (어떤 데이터라도) 임의 값을 고정 길이로 변환하는 것
- 해쉬 테이블(Hash Table): 키 값의 연산에 의해 직접 접근이 가능한 데이터 구조
    - 데이터를 저장할 공간
- 해싱 함수(Hashing Function): Key에 대해 산술 연산을 이용해 데이터 위치를 찾을 수 있는 함수
- 해쉬 값(Hash Value) 또는 해쉬 주소(Hash Address): Key를 해싱 함수로 연산해서, 해쉬 값을 알아내고, 이를 기반으로 해쉬 테이블에서 해당 Key에 대한 데이터 위치를 일관성있게 찾을 수 있음
- 슬롯(Slot): 한 개의 데이터를 저장할 수 있는 공간
- 저장할 데이터에 대해 Key를 추출할 수 있는 별도 함수도 존재할 수 있음

![https://www.fun-coding.org/00_Images/hash.png](https://www.fun-coding.org/00_Images/hash.png)

```kotlin
hash_table = list([i for i in range(10)])
hash_table
```

우리가 정의한 해시 펑션

```kotlin
def hash_func(key):
    return key % 5
```

```kotlin
data1 = 'Andy'
data2 = 'Dave'
data3 = 'Trump'
data4 = 'Anthor'
## ord(): 문자의 ASCII(아스키)코드 리턴
print (ord(data1[0]), ord(data2[0]), ord(data3[0]))
print (ord(data1[0]), hash_func(ord(data1[0])))
print (ord(data1[0]), ord(data4[0]))
```

키 값으로 첫문자의 ord()를 거쳐 나온 값 사용 (첫 문자의 아스키 코드)

데이터를 저장하는 함수

```python
def storage_data(data, value):
    key = ord(data[0])
    hash_address = hash_func(key)
    hash_table[hash_address] = value
```

```python
storage_data('Andy', '01055553333')
storage_data('Dave', '01044443333')
storage_data('Trump', '01022223333')
```

```python
def get_data(data):
    key = ord(data[0])
    hash_address = hash_func(key)
    return hash_table[hash_address]
```

```python
get_data('Andy')
```

```python
'01055553333'
```

- data의 첫번째 문자를 아스키코드로 변환한 것을 키로 사용하여 해시함수에 전달하고 해시함수가 뱉은 값을 리스트 인덱스로 사용해서 데이터에 해당하는 값(value)를 저장한다.
- get_data 에서는 data를 전달해주면 해당 데이터의 키를 구하여 해시 함수를 이용해 인덱스를 찾고 data에 해당하는 value를 반환하게된다.

### 예제 에서 해시 테이블이 아닌 배열을 썼다면?

- 리스트를 순차적으로 돌면서 data에 해당하는 값을 찾아야함
- data에 해당하는 value 값을 저장하는 배열도 함께 필요할 수도 있음

## 4. 자료 구조 해쉬 테이블의 장단점과 주요 용도

- 장점
    - 데이터 저장/읽기 속도가 빠르다. (검색 속도가 빠르다.)
    - 해쉬는 키에 대한 데이터가 있는지(중복) 확인이 쉬움
- 단점
    - 일반적으로 저장공간이 좀더 많이 필요하다.
    - **여러 키에 해당하는 주소가 동일할 경우 충돌을 해결하기 위한 별도 자료구조가 필요함**
- 주요 용도
    - 검색이 많이 필요한 경우
    - 저장, 삭제, 읽기가 빈번한 경우
    - 캐쉬 구현시 (중복 확인이 쉽기 때문)
        - 이미 캐시에 있는지 없는지

## 5. 프로그래밍 연습

연습1

- 해쉬함수 key % 8
- 해쉬 키 생성 : hash(data)

```python
hash_table = list([0 for i in range(8)])

def get_key(data):
    return hash(data)

def hash_function(key):
    return key % 8

def save_data(data, value):
    hash_address = hash_function(get_key(data))
    hash_table[hash_address] = value
    
def read_data(data):
    hash_address = hash_function(get_key(data))
    return hash_table[hash_address]
```

### **6. 충돌(Collision) 해결 알고리즘 (좋은 해쉬 함수 사용하기)**

> 해쉬 테이블의 가장 큰 문제는 충돌(Collision)의 경우입니다. 이 문제를 충돌(Collision) 또는 해쉬 충돌(Hash Collision)이라고 부릅니다.

크게 전략이 2가지로 나뉜다.

1. 오픈 해슁 기법 (추가 데이터 저장공간을 사용)
2. 클로즈 해슁 기법 (해쉬 테이블 내부의 빈공간에 저장하는 전략)

### **6.1. Chaining 기법**

- **개방 해슁 또는 Open Hashing 기법** 중 하나: 해쉬 테이블 저장공간 외의 공간을 활용하는 기법
- 충돌이 일어나면, 링크드 리스트라는 자료 구조를 사용해서, 링크드 리스트로 데이터를 추가로 뒤에 연결시켜서 저장하는 기법
    - (~~Q. 2개 이상 저장 되면 어떻게 맞는 데이터인지 구분함?~~)
        - 각각의 데이터를 구분할 수 있는 추가 정보 필요
        - 그래서 key 값을 같이 넣어주고. key값과 맞는 값을 건내준다.
        - 따라서 충돌 난 경우에는 링크드 리스트 내에서 순차 검색을 해야함

**연습2: 연습1의 해쉬 테이블 코드에 Chaining 기법으로 충돌해결 코드를 추가해보기**

1. 해쉬 함수: key % 8

2. 해쉬 키 생성: hash(data)

```python
hash_table = list([0 for i in range(8)])

def get_key(data):
    return hash(data)

def hash_function(key):
    return key % 8

def save_data(data, value):
    index_key = get_key(data)
    hash_address = hash_function(index_key)
    if hash_table[hash_address] != 0:
        for index in range(len(hash_table[hash_address])):
						# 해당 키가 이미 저장되어 있으면 덮어 씌우기
            if hash_table[hash_address][index][0] == index_key:
                hash_table[hash_address][index][1] = value
                return
				# 새로 입력된느 값이라면 뒤에 append
        hash_table[hash_address].append([index_key, value])
    else:
				# 최초 저장 시 리스트 형태로 저장
        hash_table[hash_address] = [[index_key, value]]
    
def read_data(data):
    index_key = get_key(data)
    hash_address = hash_function(index_key)

    if hash_table[hash_address] != 0:
        for index in range(len(hash_table[hash_address])):
						# 키 값과 일치하는 데이터가 있을 때만 반환
            if hash_table[hash_address][index][0] == index_key:
                return hash_table[hash_address][index][1]
        return None
    else:
        return None
```

```python
print (hash('Dave') % 8)
print (hash('Dd') % 8)
print (hash('Data') % 8)

```

```python
0
2
2
```

```python
save_data('Dd', '1201023010')
save_data('Data', '3301023010')
read_data('Dd')
```

```python
'1201023010'
```

```python
hash_table
```

```python
[0,
 0,
 [[1341610532875195530, '1201023010'], [-9031202661634252870, '3301023010']],
 0,
 0,
 0,
 0,
 0]
```

### **6.2. Linear Probing 기법 (Close hashing)**

- **폐쇄 해슁 또는 Close Hashing 기법** 중 하나: 해쉬 테이블 저장공간 안에서 충돌 문제를 해결하는 기법
- 충돌이 일어나면, 해당 hash address의 다음 address부터 맨 처음 나오는 빈공간에 저장하는 기법 (빈공간을 가만두지 않고 활용)
    - 저장공간 활용도를 높이기 위한 기법
    - 키와 함께 저장한다.

**연습3: 연습1의 해쉬 테이블 코드에 Linear Probling 기법으로 충돌해결 코드를 추가해보기**

1. 해쉬 함수: key % 8

2. 해쉬 키 생성: hash(data)

```python
hash_table = list([0 for i in range(8)])

def get_key(data):
    return hash(data)

def hash_function(key):
    return key % 8

def save_data(data, value):
    index_key = get_key(data)
    hash_address = hash_function(index_key)
		# 충돌 한다면,
    if hash_table[hash_address] != 0:
				# 순차적으로 비어있는 곳을 찾은뒤 그곳에 저장
        for index in range(hash_address, len(hash_table)):
            if hash_table[index] == 0:
                hash_table[index] = [index_key, value]
                return
						# 이미 있는 키 값에 저장 시 덮어쓰기
            elif hash_table[index][0] == index_key:
                hash_table[index][1] = value
                return
    else:
        hash_table[hash_address] = [index_key, value]

def read_data(data):
    index_key = get_key(data)
    hash_address = hash_function(index_key)
    
    if hash_table[hash_address] != 0:
        for index in range(hash_address, len(hash_table)):
						# 탐색시 비어있는 공간이 나타나면 저장되지 않은 값
            if hash_table[index] == 0:
                return None
						# 키와 일치하는 값이 있는 경우 탐색 성공
            elif hash_table[index][0] == index_key:
                return hash_table[index][1]
    else: # 0이면 데이터가 없는 것이므로 None 반환
        return None
```

```python
print (hash('dk') % 8)
print (hash('da') % 8)
print (hash('dc') % 8)
```

```python
4
4
4
```

```python
save_data('dk', '01200123123')
save_data('da', '3333333333')
read_data('dc')
```

- 주의: python hash는 실행 시마다 값 변화될 수 있음.

### **6.3. 빈번한 충돌을 개선하는 기법**

- 해쉬 함수을 재정의 및 해쉬 테이블 저장공간을 확대
- 예:

```python
hash_table= list([Nonefor iin range(16)])

def hash_function(key):
	return key % 16
```

### **참고: 해쉬 함수와 키 생성 함수**

- 파이썬의 hash() 함수는 실행할 때마다, 값이 달라질 수 있음
- 유명한 해쉬 함수들이 있음: SHA(Secure Hash Algorithm, 안전한 해시 알고리즘)
    - 어떤 데이터도 유일한 고정된 크기의 고정값을 리턴해주므로, 해쉬 함수로 유용하게 활용 가능

SHA-1

```python
import hashlib

data = 'test'.encode() # byte로 변환
hash_object = hashlib.sha1() # sha1 객체 생성
hash_object.update(data) # encode 한 str 데이터를 update에 넘겨줌
hex_dig = hash_object.hexdigest() # 해시 객체에서 해당 해시값 가져옴
print (hex_dig) # a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
```

SHA-256

```python
import hashlib

data = 'test'.encode()
hash_object = hashlib.sha256()
hash_object.update(data)
hex_dig = hash_object.hexdigest()
print (hex_dig) # 9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08
```

**연습4: 연습2의 Chaining 기법을 적용한 해쉬 테이블 코드에 키 생성 함수를 sha256 해쉬 알고리즘을 사용하도록 변경해보기**

1. 해쉬 함수: key % 8

2. 해쉬 키 생성: hash(data)

```python
import hashlib

hash_table = list([0 for i in range(8)])

def get_key(data):
        hash_object = hashlib.sha256()
        hash_object.update(data.encode())
        hex_dig = hash_object.hexdigest()
        return int(hex_dig, 16) # 16 진수 문자열을 10진수 정수로 변환

def hash_function(key):
    return key % 8

def save_data(data, value):
    index_key = get_key(data)
    hash_address = hash_function(index_key)
    if hash_table[hash_address] != 0:
        for index in range(hash_address, len(hash_table)):
            if hash_table[index] == 0:
                hash_table[index] = [index_key, value]
                return
            elif hash_table[index][0] == index_key:
                hash_table[index][1] = value
                return
    else:
        hash_table[hash_address] = [index_key, value]

def read_data(data):
    index_key = get_key(data)
    hash_address = hash_function(index_key)
    
    if hash_table[hash_address] != 0:
        for index in range(hash_address, len(hash_table)):
            if hash_table[index] == 0:
                return None
            elif hash_table[index][0] == index_key:
                return hash_table[index][1]
    else:
        return None
```

```python
print (get_key('db') % 8)
print (get_key('da') % 8)
print (get_key('dh') % 8)
```

```python
1
2
2
```

```python
save_data('da', '01200123123')
save_data('dh', '3333333333')
read_data('dh')
```

```python
'3333333333'
```

### **7. 시간 복잡도**

- 일반적인 경우(Collision이 없는 경우)는 O(1)
- 최악의 경우(Collision이 모두 발생하는 경우)는 O(n)

> 해쉬 테이블의 경우, 일반적인 경우를 기대하고 만들기 때문에, 시간 복잡도는 O(1) 이라고 말할 수 있음

### **검색에서 해쉬 테이블의 사용 예**

- 16개의 배열에 데이터를 저장하고, 검색할 때 O(n)
- 16개의 데이터 저장공간을 가진 위의 해쉬 테이블에 데이터를 저장하고, 검색할 때 O(1)