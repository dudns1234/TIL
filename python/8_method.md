# 메소드
## 문자열 메소드
- **.capitalize()** : 맨 앞 대문자
```python
a = 'hello'
a.capitalize()
'Hello' # 출력값
print(a) 
hello # 출력값 : 원본은 그대로 있음
```
- **.title()** : 각 문자별 대문자
```python
a = 'hello my name is yujin'
a.capitalize()
'Hello my name is yujin' # 출력값
a.title()
'Hello My Name Is Yujin' # 출력값
```
- **.upper()** : 전체 대문자로 변경
- **.lower()** : 전체 소문자로 변경
- **join(iterable)** : 붙이기
```python
my_list = ['hi','my','name']
''.join(my_list)
'hi my name' # 출력값
','.join(my_list)
'hi,my,name' #출력값
```
- **.replace(old, new)**
```python
'woooooooooooooow'.replace('o','!')
'w!!!!!!!!!!!!!!w' # 출력값
'woooooooooooooow'.replace('o','!',3)
'wo!!!oooooooooow' # 출력값
```
- **.strip([chars])**
```python
my_string = '        hello\n'
print(my_string)
print(my_string.strip())  # 공백지우기
# 아래 출력값
        hello

hello
```
- **.rstrip(), .lstrip()** : right, left 지우기
```python
my_string2 = 'hihihihihellohihihihi'
print(my_string2.strip('hi')) # 출력값 : ello
print(my_string2.lstrip('hi')) # 출력값 : ellohihihihi
print(my_string2.rstrip('hi')) # 출력값 : hihihihihello
```

- **.find(x)** : 원하는 인덱스 찾기 ( 없으면 음수)
```python
a = 'apple'
print(a.find('p'))  #해당하는 인덱스 출력 : 1
print(a.find('z')) # 없으면 음수로 반환 : -1
```

- **.index(x)** : 원하는 인덱스 찾기 (없으면 에러)
```python
a = 'apple'
print(a.index('p'))  #해당하는 인덱스 출력
print(a.index('z')) # 없으면 에러
```

- **.split(x)** : 쪼개기
```python
a = 'my name is Jeong'
a.split() # 기본적으로 띄어쓰기를 기준으로 split
['my', 'name', 'is', 'Jeong'] # 출력값
```
- **.count(x)** : 세기
```python
'woooooow'.count('o')
6 # 출력값
```
## 리스트 메소드
- **.append(x)** : 리스트에 값 1개 추가
- **.extend(iterable)** : 리스트에 값 여러개 추가    
`numbers = [1,2,3,4,5,6,3,4,4,1]`
```python
a = [99, 100]
numbers.extend(a)
print(numbers)
[1, 2, 3, 4, 5, 6, 3, 4, 4, 1, 10, 99, 100]
print(numbers + a)
[1, 2, 3, 4, 5, 6, 3, 4, 4, 1, 10, 99, 100, 99, 100]
```
- **.insert(idx,x)** : 해당하는 인덱스(idx)에 x값 추가
```python
numbers.insert(3, 3.5)
print(numbers)
[1, 2, 3, 3.5, 4, 5, 6, 3, 4, 4, 1, 10, 99, 100, 99, 100] # 출력값
```
- **.remove(x)** : x에 해당하는 값 제거
```python
numbers.remove(3.5)
print(numbers)
[1, 2, 3, 4, 5, 6, 3, 4, 4, 1, 10, 99, 100, 99, 100] # 출력값
```
- **.pop(x)** : x번째 인덱스 값 제거
```python
numbers.pop(0)
print(numbers)
[2, 3, 4, 5, 6, 3, 4, 4, 1, 10, 99, 100, 99, 100]
```
- **.sort()** : 리스트 값 정렬
```python
numbers.sort()
print(numbers)
[1, 2, 3, 3, 4, 4, 4, 5, 6]
numbers.sort(reverse=True) #역순
print(numbers)
[6, 5, 4, 4, 4, 3, 3, 2, 1]
```
- **.reverse()** : 리스트 값 역순으로 정렬
```python
print(numbers)
[1, 2, 3, 3, 4, 4, 4, 5, 6] # 출력값
numbers.reverse()
print(numbers)
[6, 5, 4, 4, 4, 3, 3, 2, 1] # 출력값

numbers = numbers[::-1] #슬라이싱으로 역순 (numbers[(0생략):(-1생략):-1]을 의미함.)
print(numbers)
[1, 2, 3, 3, 4, 4, 4, 5, 6] # 출력값
```
### 2.1 리스트 copy
- 문제1
```python
origin_list = [1,2,3]
copy_list = origin_list
copy_list[0] = 100
print(origin_list) -> [1, 2, ,3] 
print(copy_list) -> [1, 2, ,3]
# 분명 copy_list의 0번째만 바꿨는데 origin_list 0번째도 바뀜
# 이유 : 리스트의 주소가 같기 때문임
```

- 위 문제 해결방법 1
```python
a = [1,2,3]
b = a[:] # 복제(slicing 사용)

b[0] = 100

print(a) -> [1, 2, 3] 
print(b) -> [100, 2, 3] # 주소가 다름

```
- 위 문제 해결방법 2
```python
a = [1,2,3]
b = list(a) # 복제(list 메소드 사용)

b[0] = 100

print(a) -> [1, 2, 3]
print(b) -> [100, 2, 3] # 주소가 다름
```
- 문제2
```python
a = [1,2,[3,4]]
b = a[:]

b[2][0] = 100

print(a) -> [1, 2, [100, 4]]
print(b) -> [1, 2, [100, 4]]
# 왜 origin도 바뀐거지? 1차원의 데이터는 주소가 다른데 2차원의 데이터인 [3,4]는 주소가 같기때문에
```
- 위 문제 해결방법 1
```python
import copy
a = [1,2,[3,4]]
b = copy.deepcopy(a)

b[2][0] = 100

print(a) -> [1, 2, [3, 4]]
print(b) -> [1, 2, [100, 4]]
```

### 2.2 리스트 comprehension
`numbers = list(range(1,11))`
- for문 버전
```python
result = []
for i in numbers:
    result.append(i ** 3)
print(result)
```

- comprehension 버전
```python
result2 = [i ** 3 for i in numbers] 
print(result2)
```

- ex. 짝수만 고르기   
`numbers = list(range(1,11))`
    - for문
        ```python
        even_list = []
        for i in numbers:
            if i % 2 == 0:
                even_list.append(i)

        print(even_list)
        ```
    - comprehension
        ```python
        even_list2 = [i for i in numbers if i % 2 == 0]   # 1.for문 2.if문 3.결과를 i에 넣어줌
        print(even_list2)
        ```

- ex. 모음제거   
`words =  'my name is yujin'  ,vowels = 'aeiou'`
    - for문   
        ```python
        result =''
        for i in words:
            if i not in vowels:
                result += i

        print(result)
        ```
    - comprehension
        ```python
        result2 = [i.upper() for i in words if i not in vowels]
        print(result2)
        print(''.join(result2))
        ```
## 3. 딕셔너리 메소드
`
info = {
    'name' : 'yujin',
    'location' : 'seoul'
}
`
- **.pop(key[,default])** : key 값 제거
```python
info.pop('location')
print(info) -> {'name': 'yujin'} # 출력값
print(info.pop('location',None)) -> None # 출력값
print(info.pop('location','없습니다')) -> '없습니다' # 출력값

```

- **.update(key = value)**
```python
info.update(name = 'Kim')
print(info)  -> {'name': 'kim'} # 출력값
```

- **.get(key[,default])**
```python
print(info.get('name')) -> Kim # 출력값
print(info.get('school')) -> None(기본값) # 출력값
print(info.get('school','없다')) -> 없다 # 출력값
print(info['school']) -> error # 출력값
```

### 3.1 dict comprehension
- ex. {1 : 1의 3제곱, 2 : 2의 3제곱, 3 : 3의 3제곱 ...}

- for문
```python
cube_dict = {}
for i in range(1,4):
    cube_dict[i] = i ** 3
print(cube_dict) -> {1: 1, 2: 8, 3: 27} # 출력값
```

- comprehension
```python
cube_dict2 = {i: i**3 for i in range(1,4)}
print(cube_dict2) -> {1: 1, 2: 8, 3: 27} # 출력값
```
- ex2. dust값이 50 이상인 곳을 추출    
`dust = {
    '서울' : 100,
    '대구' : 30,
    '대전' : 10,
    '부산' : 80,
    '광주' : 50
}
`
- for문
```python
result ={}
for k,v in dust.items():
    if v >= 50:
        result[k] = v

print(result)
```

- comprehension
```python
result2 = {k: v for k,v in dust.items() if v >= 50}
print(result2)
```

## 세트 메소드

`fruits = {'apple','banana','melon'}`

- **.add(x)** : 한개 추가
```python
fruits.add('watermelon')  # 순서가 없다, 중복도 없다
print(fruits)
```

- **.update()** : 여러개 추가
```python
fruits.update('grape')
print(fruits)
{'banana', 'watermelon', 'g', 'r', 'a', 'melon', 'e', 'p', 'apple'} # 출력값
fruits.update({'orange' ,'pear'})
print(fruits)
{'banana', 'watermelon', 'g', 'r', 'a', 'orange', 'melon', 'e', 'pear', 'p', 'apple'} # 출력값
```

- **.remove()** : 값 지우기
```python
fruits.remove('orange')
print(fruits)
{'banana', 'watermelon', 'g', 'r', 'a', 'melon', 'e', 'pear', 'p', 'apple'} # 출력값
```

- **.pop()** : 앞에서부터 값 지우기
```python
fruits.pop()
print(fruits)
{'watermelon', 'g', 'r', 'a', 'melon', 'e', 'pear', 'p', 'apple'} # 출력값
```

