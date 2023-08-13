### lambda 표현식

```python
lambda parameter : expression
```

예시
```python
(lambda a, b: a+b)(1,2)  #lambda = def, a, b : (a,b), a+b : return a+b

# 위와 아래 같은 식

def my_sum(a,b):
    return a + b

my_sum(1,2)
```

### 타입 힌트
- 함수 어노테이션
- 상세하게 어떤 타입이 들어오는지
- 함수를 쓰려고 하는 개발자를 위해서 타입을 주석으로 달아준 개념

```python
# 개발자들의 편의성을 위해 설명을 적는것 1
def my_sum(num1 : int, num2 :int) -> int:  
    return num1+num2

my_sum(1,2)
my_sum('1',2)
```

```python
# 개발자들의 편의성을 위해 설명을 적는것 2
def my_sum(num1 : int, num2 :int) -> int:
    '''
    두수의 합을 구하는 함수입니다.
    매개변수 num1, num2를 받아서 num1+num2를 리턴입니다.  
    '''
my_sum('1',2)
```

### 이름공간(scope)
파이썬에서 사용되는 이름들은 이름공간(namespace)에 저장되어있습니다.
- Local scope : 정의된 함수 내부
- Enclosed scope : 상위 함수
- Global scope : 함수 밖의 변수 혹은 import된 모듈
- Built-in scope : 파이썬이 기본적으로 가지고 있는 함수 혹은 변수
```python
str = '123' #local scope
str(456) # Built-in scope
```
```python
a = 1
def localscope(a):
    # a = 5  # localscope(5)
    print(a) # 함수내에서는 함수 안에있는 변수가 나오게됨

localscope(5)
5 # 출력값
```