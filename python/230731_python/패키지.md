## module
import fibo
print(fibo)
fibo.fib_loop(4)
fibo.fib_rec(3)

## 패키지
- 패키지 안에 `__init__.py` 파일이 있어야 패키지로 인식

```python
mypackage/
    __init__.py
    math/
        __init__.py
        fibo.py
        formula.py
```


```python
# 패키지 폴더 전체 가져옴
import myPackage

print(myPackage) -> <module 'myPackage' from 'C:\\Users\\yujin\\Desktop\\camp29\\python\\myPackage\\__init__.py'>  # 출력값

#패키지에서 필요한 모듈을 꺼내오는 코드
from myPackage.math import formula  # formula라는 파일로 접근
formula.pi -> 3.14 # 출력값

# 경로에 있는 모듈이 가지고 있는 모든 변수, 함수를 추가
from myPackage.math.fibo import *

# 변수명과 모듈명이 같을 때 as를 사용하여 모듈 별명 설정
formula = 1234
from myPackage.math import formula as f

print(formula) -> 1234 # 출력값
print(f) -> <module 'myPackage.math.formula' from 'C:\\Users\\yujin\\Desktop\\camp29\\python\\myPackage\\math\\formula.py'> # 출력값
 ```

## 파이썬 내장 패키지
### math
`import math`
```python
math.pi -> 3.1415926536... # 출력값
math.e -> 2.718281828... # 출력값
math.ceil(pi) -> 4 # 소수점 올림
math.floor(pi) -> 3 # 소수점 내림
math.sqrt(9) -> 3.0 # 루트
```

### random
`import random`
```python
random.random() # 소수
random.randint() # 정수
random.seed(1) # 고정값

# 리스트 섞을때
a = [1,2,3,4,5]
random.shuffle(a) 
print(a)

# choice - 중복 O
a = [1,2,3,4,5]
print(random.choice(a))

# sample - 중복 X
a = range(1,46)
random.sample(a,6)
```

### datetime
`from datetime import datetime`
```python
now  = datetime.now()
print(now) -> 2023-07-31 16:21:04.110400 # 출력값

today = datetime.today()
print(today) -> 2023-07-31 16:21:29.741927 # 출력값

utc = datetime.utcnow()
print(utc) -> 2023-07-31 07:23:49.459180 # 출력값

print(now.strftime('%Y년 %m월 %d일')) -> 2023년 07월 31일 # 출력값
print(now.strftime('%Y/%m/%d')) -> 2023/07/31 # 출력값

now.year -> 2023 # 출력값
now.day -> 31 # 출력값

# weekday => 0~6, 월~일
now.weekday()  -> 0 # 출력값

birth = datetime(2023, 1, 1)
print(birth)
```

`from datetime import timedelta`
```python
future = timedelta(days = 3)
print(future) -> 3 days, 0:00:00 # 출력값

birth + future -> datetime.datetime(2023, 1, 4, 0, 0) # 출력값
```