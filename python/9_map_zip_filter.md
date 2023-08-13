## 5. map(), zip(), filer()
### 5.1 map()
`map(function, iterable)`

```python
a = [1,2,3] # int

number_str = map(str, a)
print(number_str) -> <map object at 0x00000200FFB986D0> # 출력값 : 객체만 만들었기 때문에 주소가 출력

print(list(number_str)) -> ['1', '2', '3'] # 출력값 : 연산이 실행이 되어 결과가 나옴
```

### 5.2 zip()
`a = [1, 2, 3] b = [100, 200, 300] `
```python
result = zip(a,b)
print(result) -> <zip object at 0x0000020080FA3B80> # 출력값
print(list(result)) -> [(1, 100), (2, 200), (3, 300)] # 출력값
```

### 5.3 filter()
`filter(function, iterable)`
- filter에 들어가는 function은 T/F를 반환해야 합니다.

```python
def is_odd(x):
    return bool(x % 2) # 1(True) , 0(False)
```

```python
numbers = [1,2,3,4,5]
result = filter(is_odd, numbers)
print(result) -> <filter object at 0x000002008109D3C0> # 출력값
print(list(result)) -> [1, 3, 5] # 출력값
```