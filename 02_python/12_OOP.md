# 객체지행 프로그래밍(OOP)

- 클래스(class) : 같은 종류의 집단에 속하는 속성(attribute)과 행위(method)를 **정의**한 것
- 인스턴스(instance) : 클래스를 실제로 메모리상에 할당한 것
- 속성(attribute) : 클래스 / 인스턴스가 가지고 있는 **데이터**/값
- 행위(method) : 클래스 / 인스턴스가 가지고 있는 **함수**/값

- 객체 : 클래스 + 인스턴스

```python
number = 1 + 2j
print(type(number))
<class 'complex'> # 출력값
```

```python
# number로 사용할 수 있는 속성들 확인
dir(number) -> real, imag, conjugate # 출력값

# dir()에서 확인했을 때 사용가능했던 속성
- 속성은 데이터이기 때문에 뒤에 ()를 붙이지 않음.
print(number.real) 
print(number.image)
```

```python
# my_list 객체 생성
my_list = [1,2,3,4,5]
# sort 행위(method)
my_list.sort() 
```

- ex. 휴대폰 사용자 관리
```python
# 대상자 1, 2, 3, ...
power = False
number = '010-1234-1234'
book = {
    '홍길동' : '010-1111-1111',
    '이순신' : '010-2222-2222'
}
model = 'iPhone12'

def on():
    global power
    if power == False:
        power = True
        print('핸드폰 켜짐')

on() -> 휴대폰 켜짐 # 출력값
```
Q. 점점 대상자2, 대상자3, ..., 대상자10000 로 되면 관리하기가 힘들어지기 때문에 이와 같은 문제를 해결하기 위해 **객체지향 프로그래밍**이 등장함.

## class

```python
 # 클래스 선언
class ClassName:
    attribute = value

    def method_name(self):
        code

# 인스턴스화
ClassName()
```

#### - 선언
```python
class MyClass:
    name = 'kim'

    def hello(self):
        return 'hello'
```
#### - 인스턴스화
```python
a = MyClass()
```
```python
print(a.name)  # 속성 (attribute)
kim # 출력값
print(a.hello()) # 행위 (method) 
hello # 출력값
```

**예시**
```python
# class 선언
class Phone:
    power = False
    number = '010-0000-0000' # 속성/데이터
    book = {} # 속성/데이터
    model = '' # 속성/데이터

    def on(self):  # 행위(method)/function
        if self.power == False:
            self.power = True

    def off(self):  # 행위(method)/function
        if self.power == True:
            self.power = False

    def call(self, target):  # 행위(method)/function
        if self.power == True:
            print(f'내 번호는 {self.number}입니다.')
            print(f'{target}번호로 전화거는중')
        else:
            print('핸드폰을 켜주세요')
```
```python
# 인스턴스화
my_phone = Phone()
your_phone = Phone()
```
```python
# 속성/메소드 출력
my_phone.number 
'010-0000-0000' # 출력값

my_phone.number = '010-1234-1234'
my_phone.number
'010-1234-1234' # 출력값

your_phone.number
'010-0000-0000' # 출력값

my_phone.power
False  # 출력값

my_phone.on()
my_phone.power
True  # 출력값

my_phone.call('112')
내 번호는 010-1234-1234입니다.  # 출력값
112번호로 전화거는중
```

#### 정리
```python
class Person: # 클래스 정의(선언) : 클래스 객체 생성
    name = 'kim' # 속성(attribute) : 변수/값/데이터

    def hello(self) : # 행동(method) : 함수/기능
        return self.name

p = Person() # 인스턴스화 : 인스턴스 객체를 생성
p.name  # 속성을 호출
p.hello() # 메소드를 실행
```
#### self 관련

- self : 인스턴스 객체 자기자신 (다른언어에서는 this)
- 특별한 상황을 제외하고는 무조건 메소드의 첫번째 인자로 설정한다.
- 인스턴스 메소드를 실행할 때 자동으로 첫번째 인자에 인스턴스를 할당한다.



## 생성자, 소멸자

```python
class MyClass:
    def __init__(self):
        pass
    
    def __del__(self):
        pass
```

```python
class Person:
    name = 'noname'
    
    def __init__(self, name (= '익명')):  # def __init__(self, name = '익명') 이렇게도 가능.
        self.name = name
        print('생성됨')
    
    def __del__(self):  # 소멸자
        print('소멸됨')
```

```python
p1 = Person()  # =>  Person.__init__(p1,)
print(p1.name)
p1.name = 'yujin'
print(p1.name)
익명   # 출력값
생성됨 # 출력값
yujin # 출력값

p2 = Person()
del p2
생성됨 # 출력값
소멸됨 # 출력값
```



### 클래스 변수
- 클래스 선언 블록 최상단에 위치

### 인스턴스 변수
- 인스턴스 내부에서 생성한 변수 (self.variable = )

```python
class TestClass:
    class_variable = '클래스변수'

    def __init__(self, arg):
        self.instance_variable = '인스턴스변수'

    def status(self):
    return self.instance_variable
```

```python
# Person class
class Person:
    name = '홍길동'
    phone = '010-1234-1234'
    
    def __init__(self, name):
        self.name = name

# Person instance
p = Person('정유진')
print(Person.name) # 클래스 변수
홍길동 # 출력값
print(p.name) # 인스턴스 변수
정유진 #출력값
print(p.phone) # p(나)에 없다면 Person(부모)로 가게됨 if 부모도 없으면 error
010-1234-1234 # 출력값
```

### 클래스 메소드, 인스턴스메소드, 스태틱메소드

```python
class MyClass:
    def instance_method(self):
        pass
    
    @classmethod
    def class_method(cls):
        pass

    @staticmethod
    def static_method():
        pass

```
- class 선언
```python
class MyClasas:
    def instance_method(self):
        return self

    @classmethod
    def class_method(cls):
        return cls

    @staticmethod
    def static_method():
        return 'hello'
```
- 인스턴스화 및 메소드 출력
```python
c1 = MyClass()

print(c1.instance_method()) -> <__main__.MyClass object at 0x000002527607B490> # 출력값
print(MyClass.class_method()) -> <class '__main__.MyClass'>  # 출력값
print(c1.class_method()) -> <class '__main__.MyClass'>  # 출력값
print(c1.static_method()) -> hello # 출력값 
```
- class 선언
```python
class Puppy:
    num_of_puppy = 0

    def __init__(self, name):
        self.name = name
        Puppy.num_of_puppy += 1

    @classmethod
    def get_status(cls):
        return f'현재 강아지는 {cls.num_of_puppy}마리 입니다.'

    @staticmethod
    def bark(string = '멍멍'):
        return string
```
- 인스턴스화 및 데이터/메소드 출력
```python
p1 = Puppy('또또')
p2 = Puppy('몽이')
p3 = Puppy('흰둥이')

print(p1.num_of_puppy)  -> 3  # 출력값
print(p2.num_of_puppy)  -> 3  # 출력값 
print(Puppy.num_of_puppy)  -> 3  # 출력값

print(Puppy.get_status()) -> 현재 강아지는 3마리입니다. # 출력값

print(p1.bark()) -> 멍멍 # 출력값
print(p2.bark('그르릉')) -> 그르릉 # 출력값
```
## 총정리
```python
class
    - attrubute (variable, data)
        - instance_variable
        - class_variable
    - method
        - instance_method
        - class_method
        - static_method
```

## 상속
- 부모
```python
class Person:

    def __init__(self, name):
        self.name = name
        
    def greeting(self):
        print(f'안녕하세요 {self.name}입니다.')
```

```python
p1 = Person('홍길동')
p2 = Person('이순신')

p1.greeting() -> 안녕하세요 홍길동입니다 # 출력값
```

- 상속
```python
class Student(Person):  # 상속
    
    # def __init__(self, name)  : 부모한테 가져온 init
    #     self.name = name
        
    # def greeting(self):
    #     print(f'안녕하세요 {self.name}입니다.')
    
    def __init__(self, name, student_id):  # 내 자신 init (먼저 실행됨)
        self.name = name
        self.student_id = student_id
    pass

class Soldier(Person):

    def greeting(self):
        return f'충성! {self.name}입니다.'
```

```python
s1 = Student('kim',12345)
s2 = Student('park',98765)

s1.greeting()
안녕하세요 kim입니다. # 출력값

print(s1.student_id)
12345 # 출력값

s1 = Soldier('국방이')
s1.greeting()
'충성! 국방이입니다.' # 출력값
```
```python
class Person:
    def __init__(self, email, phone, location, name):
        self.email = email
        self.phone = phone
        self.location = location
        self.name = name

class Student(Person):
    def __init__(self, email, phone, location, name, student_id):
        self.email = email
        self.phone = phone
        self.location = location
        self.name = name
        self.student_id = student_id

class Soldier(Person):
    def __init__(self, email, phone, location, name, soldier_id):
        # super()  : 부모 클래스를 가리키게됨 (Person)
        super().__init__(email, phone, location, name)
        self.soldier_id = soldier_id
```

```python
s1 = Soldier('email@email.com','010-1234-2344','seoul','kim','12345')
print(s1.name) -> kim # 출력값
print(s1.soldier_id) -> 12345 # 출력값
```
## 다중 상속

```python
class Person:
    def __init__(self, name):
        self.name = name
        
    def breath(self):
        print('후하')
```

```python
class Mom(Person):
    gene = 'xx'

    def swim(self):
        print('어푸어푸')
```

```python
class Dad(Person):
    gene = 'xy'

    def run(self):
        print('다다다')
```
- 다중상속 예시
```python
class Baby(Mom, Dad): # 다중 상속
    pass
```

```python
b = Baby('금쪽이')
b.breath()  -> 후하  # 출력값
b.swim()  -> 어푸어푸 # 출력값
b.run()  -> 다다다 # 출력값
print(b.gene)  -> xx  # 상속 순서가 있다 ->  class Baby(Mom, Dad)
```

- 다중상속 예시
```python
class Baby(Dad, Mom): # 다중 상속
    pass
```

```python
b = Baby('금쪽이')
b.breath()  -> 후하  # 출력값
b.swim()  -> 어푸어푸 # 출력값
b.run()  -> 다다다 # 출력값
print(b.gene)  -> xy  # 상속 순서가 있다 -> class Baby(Dad, Mom)
# 다중상속을 한 경우 먼저 상속받은 데이터/메소드가 우선
```