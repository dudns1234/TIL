## 함수(Function)
### 함수의 선언과 호출
- 함수의 선언
```python
def func_name(parameter1, parameter2...):
    code1
    code2
    ...
    return value
```

-함수의 호출(실행)
```python
func_name(parameter1, parameter2)
```

```python
# 함수의 선언
def rectangle(height, width):
    area = height * width
    perimeter = (height + width) * 2
    print(f'직사각형의 둘레는 {perimeter}, 면적은 {area} 입니다.')
```

```python
# 함수의 실행
rectangle(10,20)
rectangle(20,30)
```

### 함수의 return
- 함수가 return을 만나면 해당 값을 반환하고 함수를 종료
- 만약 return이 없다면 None을 자동으로 반환
- return은 오직 하나의 객체만 반환
- print는 할당이 불가능해서 재사용이 불가능 하지만 return은 할당을 해줘서 재사용이 가능

```python
# 두개의 정수를 받아서 큰수를 반환
def my_max2(a, b):

    #return 'hello' -> hello 출력되고 끝.
    
    if a > b :
        return a
    elif b > a:
        return b
    else:
        return 0

result = my_max2(1,5)
print(f'{result}가 더 큽니다.')