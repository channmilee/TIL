### Function
1. 함수 형태

```python
1. 인자 값 X, 리턴 값 X
def aa():
 print('hi')

aa()
> hi

2. 인자 값 O, 리턴 값 X
def my_friends(friend_name):
  print('{}은 내 친구'.format(friend_name))

my_friends('mimi')
> mimi는 내 친구

3. 인자 값 X, 리턴 값 O
def cc():
 n = int(int(input('n:')))
 print(n**2)
 return n*
 
re = cc()
> n:10
100
20

4. 인자 값 O, 리턴 값 O
def my_calc(x,y):
  z = x*y
  return z

my_calc(3,4)
>12
```
---

2. 변수의 유효 범위

-> 변수를 함수 안에서 정의했느냐, 함수 밖에서 정의했느냐에 따라 코드 내에서 영향을 미치는 유효 범위가 달라짐

   1. 지역 변수(local variable)
    - 함수 안에서 정의한 변수는 함수 영역 안에서만 동작
    - 다른 함수에서 같은 이름으로 변수를 사용해도 각각 다른 임시 저장 공간에 할당 -> 독립적으로 동작

   2. 전역 변수(global variable)
    - 코드 내 어디서나 사용 가능
  
   3. 스코핑 룰(Scoping rule), LGB 룰(Local, Global, Built_in rule)
    - 함수에서 어떤 변수를 호출하면 >지역 영역, 전역 영역, 내장 영역< 순서대로 변수가 있는지 확인
    * 내장 영역 = 파이썬 자체에서 정의한 이름 공간
    - 동일한 변수명을 지역 변수와 전역 변수에 모두 이용했다면 스코핑 룰에 따라 변수가 선택
  
```python
a = 5 #전역변수

def func1():
  a = 1 #지역변수
  print('[func1] 지역변수 a =', a)

def func2():
  a = 2 #지역변수
  print('[func2] 지역변수 a =', a)
  
def func3():
  print('[func3] 전역변수  =', a)

def func4():
  global a #전역변수 변경
  a = 4
  print('[func4] 전역변수 a =', a)

func1()
> [func1] 지역변수 a = 1

func2()
> [func2] 지역변수 a = 2

func3()
> [func3] 전역변수  = 5

func4()
> [func4] 전역변수 a = 4

func3()
> [func3] 전역변수  = 4
# func4 실행으로 전역변수가 4로 변경됨
```

---

3. lambda 함수

> lambda<인자>:<인자 활용 수행 코드>
> 
> (lambda<인자>:<인자 활용 수행 코드>)(<인자>)

```python
(lambda x : x**2) (3)
> 9

my_sqaure = lambda x : x**2
my_sqaure(3)
> 9

my_func = lambda x,y,z : 2*x + 3*y+ z
my_func(1,2,3)
> 11
```

---

4. bool 함수
```python
bool(0)
> False

bool(1)
> True

bool(' ')
> True

bool('')
> False

bool(None)
> False
```
