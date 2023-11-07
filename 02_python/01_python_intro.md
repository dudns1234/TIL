## Python
- Jupyterlab 사용

## intro
- 단축키
- 주의사항   

## 1. 변수   

변수이름 = 값
- 변수이름은 어떤 이름이든 상관 없음
- 다만 영어, 숫자, _를 이용하여 선언
- 키워드는 사용불가

### 1-1. number
### 1-2. Boolean
True, False로 이뤄진 타입
### 1-3. None
### 1-4. String
문자열을 `'`, `"` 를 이용하여 표현   
`print('안녕하세요? "정유진"입니다')`   
`print("안녕하세요? '정유진'입니다")`   
`print('안녕하세요? \'정유진\'입니다')`   
`print('엔터를 입력해봐요 \n다음줄입니다.')`   
`print('엔터를 입력해봐요 \t들여쓰기도 가능합니다.')`
`print('하나', '둘', '셋', end = '!')`
`print('하나', '둘', '셋', sep = '!')`

|string interpolation| 예시 | print |
| --- | ---| ---|
age =20 가정
%-formatting | `print('홍길동은 %s 살입니다.' %age)` | 홍길동은 20 살입니다.
str.format() | `print('홍길동은 {}살입니다.'.format(age))` | 홍길동은 20 살입니다.
f-string | `print(f'홍길동은 {age}살입니다.')` | 홍길동은 20 살입니다.


## 2. 연산자
### 2-1. 산술연산자
a + b : 덧셈   
a - b : 뺄셈   
a * b : 곱셈   
a / b : 나눗셈   
a ** b : 제곱   
a // b : a를 b를 나눈 몫   
a % b : a를 b로 나누고 나머지   
divmod(a,b) : a를 b로 나눈 몫, 나머지

### 2-2. 비교연산자
< , >, <= , >=, ==, !=

### 2-3. 논리연산자
and : 양쪽 모두 True일 때, True 반환
or : 양쪽 모두 False일 때, False 반환
not : 값을 반대로 전환

- 단축평가
> 형변환에 대해서 알아야하며, True나 False만 생각하면 안됨   
> 0 : False, 1 이상 : True로 인식

**단축평가(and)**
- `print( 3 and 5 )` #앞이 3(True)이기 때문에 뒤가 나와야함 -> 5    
- `print( 3 and 0 )` -> 0   
- `print ( 0 and 5 )` # 앞이 0(False)이기 때문에 앞이 나와야함 -> 0    
- `print ( 0 and 0 )` ->0    

**단축평가(or)**
- `print( 3 or 5 )` #앞이 3(True)이기 때문에 앞이 나와야함, -> 3    
- `print( 3 or 0 )` #  -> 3    
- `print ( 0 or 5 )` # 앞이 0(False)이기 때문에 뒤가 나와야함 -> 5    
- `print ( 0 or 0 )` -> 0

### 2-4. 복합연산자
|산술연산자| 복합연산자 |
|---|---|
a = a + b | a += b   
a = a - b | a -= b   
a = a * b | a *= b   
a = a / b | a /= b   
a = a // b | a //= b   
a = a % b | a%= b   
a = a ** b | a**= b   

### 2-5. 기타연산자
- '+' : concatenation 
- in : containment
- is : identity

|값 | 식 | 답|
|---|---|---|
a = 'hi' b = 'hello' | `a + b` | 'hihello'
a = [1, 2, 3]  b = [4, 5, 6] |  `a + b`  |  [1, 2, 3, 4, 5, 6]
--- | `print('a' in 'apple')` | True
--- | `print ( 10 in [1,2,3])` | False
a = 1 b = 1 |  `print(a is b)` | True
a = 123123 b = 123123 | `print(a is b)` | False
a = 1 b = 1 |  `print(id(a)) print(id(b))` | 140719892849448 140719892849448
a = 123123 b = 123123 | `print(id(a)) print(id(b))` | 1776377919952 1776377919696

Q. is 예시는 왜 답이 다를까?   
A. is는 메모리까지 완벽하게 같아야한다.    
작은 숫자인 1 같은 경우는 메모리까지도 동일하지만   
큰 숫자인 123123 같은 경우는 메모리가 서로 다름.

### 우선순위

0. ()를 통해 그룹
1. **
2. 산술연산자(*,/)
3. 산술연산자(+,-)
4. 비교연산자, in, is
5. not
6. and
7. or

|식 | 답|
|---|---|
`print(-3 ** 4)` | -81
`print((-3) ** 4)` | 81

## 3. 형변환
### 3-1. 암시적 형변환
|값 | 식 | 답|
|---|---|---|
a = True b =1 | `a+b` | 2
a = False b = 1 | `a+b` | 1
a = 3 b = 1+3j | `a+b` | (4+3j)

### 3-2. 명시적 형변환
- int() : string, float를 int로 변환
- float() : string, int를 float로 변환
- str() : int, float 등을 string으로 변환
- bool() : int, list 등을 boolean으로 변환   

|값 | 식 | 답|
|---|---|---|
a = 1 b = "번" | `print (a + b)` | error | 
error 이유 | int와 str를 연결하려고 했기때문 (+는 int끼리 혹은 str끼리 사용 ) 
a = 1 b = "번" | `print (str(a) + b)` | 1번

## 4. 시퀀스(Sequence) 자료형
시퀀스는 데이터의 순서대로 나열된 자료구조.    
(순서대로 나열되었다는 것은 정렬된것과 다르다)

1. List
2. Tuple
3. Range
4. String

### 4-1. List
- 선언 : 변수이름 = [value1, value2, value3 ...]
- 접근 : 변수이름[index]

ex. location = ['서울', '대구', '대전'] *#list 선언*
|식 | 답|
|---|---|
`print(location[0])` | 서울  *# list 접근*
`print(location[1])` | 대구
`location[1] = '부산'` | -
`print(location)` | ['서울', '부산', '대전']

### 4.2 Turple
- 선언 : 변수이름 = (value1, value2, value3)
- 접근 : 변수이름[index]
- 리스트와 유사하지만 수정 불가능(immutabel)하다.

ex. t = (1,2,3)
|식 | 답|
|---|---|
`print(t[2])` | 3
`t[2] = 5` | error
error 이유 | tuple 형식은 어떤 값을 할당하는 것을 지원안함

Q. 그럼 tuple은 수정도 안되는데 어디에다가 사용?   
A. 여러개의 데이터에 동시에 할당할 때 사용 (아래 참조)

|값 | 식 | 답|   
|---|---|---|
x,y = 1, 2 | `print(x,y)` | 1 2
x,y = y, x | `print(x,y)` | 2 1

### 4.3 range

- range(n) : 0부터 n-1까지 범위
- range(n, m) : n부터 m-1까지 범위
- range(n, m, s) : n부터 m-1까지 +s만큼 증가하는 범위

### 4.4 String
기본 데이터 구조 참고

### 4.5 시퀀스에서 활용 가능한 연산/구조
- slicing : `data[1]`
- indexing : `data[1:3]`
- slicing(k간격) : `data[1:3:k]`
- in, not in
- concatenation(+) , 곱하기(*)
- len, min, max, .count()

## 5. 시퀀스 데이터가 아닌 자료구조

### 5.1 Set
수학에서 사용하는 집합과 동일하게 처리
- 선언 : 변수이름 = {value1, value2, value3}

ex. my_set_a = {1,2,3,4,5} my_set_b = {3,4,5,6,7}
|식 | 답|
|---|---|
`print(my_set_a - my_set_b)` #차집합 | {1, 2}
`print(my_set_a \ my_set_b)` # 합칩합 | {1, 2, 3, 4, 5, 6, 7}
`print(my_set_a & my_set_b)` # 교집합 | {3, 4, 5}

ex. my_list = [1,1,1,1,1,1,1,2,3,4,5,6,7]
|식 | 답|
|---|---|
`print(set(my_list))`  #중복제거 | {1, 2, 3, 4, 5, 6, 7}
`print(list(set(my_list)))` | [1, 2, 3, 4, 5, 6, 7]

### 5.2 Dictionary

- 선언 : 변수이름 = {key1: value1, key2: value2, key3: value3 ...}
- 접근 : 변수이름[Key]

- dictionary는 key와 value가 쌍으로 이루어져있다.
- key에는 immutable한 모든것을 사용가능 (불변값 : string, integer ...)
- value에는 모든 데이터 가능 (list, dictionary도 가능)

ex. my_dict = {'서울': '02', '경기도': '031'}
| 식 | 답|   
|---|---|
`my_dict['서울']` | 02
`dict_a.keys()` | dict_keys(['서울', '경기도'])
`dict_a.values()` | dict_values(['02', '031'])


## 총정리

### 데이터 타입

1. Number
2. Boolean
3. String

### 자료구조
- **시퀀스 자료형**
1. [List] : 수정가능(mutable)
2. (Tuple) : 불변함(immutable)
3. range() : 불변함(immutable)
4. 'String' : 불변함(immutable)

- **시퀀스가 아닌 자료형**
1. {Set} : 수정가능(mutable)
2. {Dictionary} : 수정가능(mutable)

