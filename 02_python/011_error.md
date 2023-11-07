## Error
### synatax error
```python
if True:
    pass
else
```

```python
print('hi'
```

### Exception
- ZeroDivisionError
```python
print(5/0)
ZeroDivisionError: division by zero
```
- NameError
```python
print(my_name)
NameError: name 'my_name' is not defined
```
- TypeError
```python
print(1 + '12')
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```
```python
'1' + 1
TypeError: can only concatenate str (not "int") to str
```
- ValueError
```python
int('3.5')
ValueError: invalid literal for int() with base 10: '3.5'
```
- IndexError
```python
numbers = [1,2,3,4]
numbers[100]
IndexError: list index out of range
```
- KeyError
```python
my_dict = {
    'apple' : '사과',
    'banana' : '바나나'
}
my_dict['melon']
KeyError: 'melon'
```
- ModuleNotFoundError
```python
import asdf
ModuleNotFoundError: No module named 'asdf'
```


### 예외처리

```python
try:
    code
except 예외:
    code
```
- ex

```python
try:
    num = int(input('숫자입력:'))
    print(f'{num}의 세제곱은 {num**3} 입니다.')
except ValueError:
    print('숫자입력!')
```
```python
try:
    num = int(input('나눌값 입력:'))
    print(f'100을 {num}으로 나누면 {100/num}입니다.')
except (ValueError, ZeroDivisionError):
    print('문제발생')
```

```python
try:
    my_list = []
    print(my_list[10])
except IndexError as err:  # as : 에러 별명을 출력하여 어떠한 에러인지 출력
    print('범위문제')
    print(err)
```

```python
# else : 예외를 일으키지 않았을 때 실행되는 코드
try:
    numbers = [1,2,3]
    num = numbers[1]
except:
    print('오류발생')
else:
    print(num*100)   
```

```python
# finally : 예외의 발생 여부와 상관없이 최종적으로 무조건 실행하는 코드
try:
    info = {
        'name' : 'kim',
        'loation' : 'seoul'
    }
    #info['phone']
    info['name']
except:
    print('없는키')
finally:
    print('hello')
    
```
- 예외를 강제로 발생시키는 상황에서 사용
```python
raise
```
-  예외처리 연습
```python
def my_div(num1, num2):
    try:
        result = num1 / num2
    except ZeroDivisionError:
        return'0으로 나눌 수 없다.'
    except:
        return '숫자입력'
    else:
        return result

# print()는 중간 산출물이고 return은 결과물임.

print(my_div(5,0))
print(my_div('5','0'))
print(my_div(5,2))
```
- **except ZeroDivisionError:** 와 **except:** 의 순서   
     except:가 except ZeroDivisionError: 에러보다 포함하는 에러의 범위가 크기 때문에 ZeroDivisionError 보다는 뒤에 있어야한다.