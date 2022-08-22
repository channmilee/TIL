### class

1. 객체
   1. 속성: 변수
   2. 행위: 함수

  -> 클래스를 통해 객체 생성 = 인스턴스

2. class 선언

```python
class Bicycle():
  def move(self, speed): 
    print('자전거: 시속 {}킬로미터로 전진'.format(speed))

  def turn(self, direction):
    print('자전거: {}회전'.format(direction))
  
  def stop(self):
    print('자전거 ({},{}): 정지'.format(self.wheel_size, self.color)) 
    # 클래스 함수 안에서는 'self.변수명'으로 속성값을 가져옴

1. 객체 생성(인스턴스)
my_bicycle = Bicycle()

my_bicycle.wheel_size = 26 # 객체의 속성 설정
my_bicycle.color = 'black'

my_bicycle.move(30) # 클래스 메서드 호출
my_bicycle.turn('좌')
my_bicycle.stop()

> 자전거: 시속 30킬로미터로 전진
자전거: 좌회전
자전거 (26,black): 정지


2. 객체 초기화
- 객체를 생성하는 것과 동시에 속성값 지정
- __init__() 함수에 초기화하려는 인자를 정의하면 '객체 생성시 속성 초기화 가능'
- 'self.변수명 = 인자'로 객체의 속성 초기화

class Bicycle():

  def __init__(self, wheel_size, color):
    self.wheel_size = wheel_size
    self.color = color

  def move(self, speed):
    print('자전거: 시속 {}킬로미터로 전진'. format(speed))

  def turn(self, direction):
    print('자전거: {}회전'.format(direction))
  
  def stop(self):
    print('자전거 ({},{}): 정지'.format(self.wheel_size, self.color))


my_bicycle = Bicycle(26, 'black') # 객체 생성과 동시에 속성값 지정

my_bicycle.move(30)
my_bicycle.turn('좌')
my_bicycle.stop()
```


3. 클래스에서 사용하는 변수
  1. 클래스 변수
  - 클래스 내에 있지만 함수 밖에서 '변수명 = 데이터' 형식으로 정의
  - 클래스에서 생성한 모든 객체가 공통으로 사용
  - **'클래스 명.변수'** 형식으로 접근
  - 객체 생성 후 **'객체명.변수명'** 형식으로 접근 가능

  2. 인스턴스 변수
  - 클래스 내 함수 안에서 'self.변수명 = 데이터' 형식으로 정의
  - 클래스 내 모든 함수에서 **'self.변수명'** 으로 접근
  - 인스턴스 변수는 각 인스턴스(객체)에서 개별적으로 관리
  - 객체를 생성한 후에 **'객체명.변수'** 형식으로 접근
  - 인스턴스 변수가 정의돼 있지 않고 클래스 변수만 정의돼 있을 때 객체를 생성한 후 '객체명.변수'로 접근하면 클래스 변수에 접근

```python
class Car():
  instance_count = 0   # 클래스 변수 생성 및 초기화

  def __init__(self, size, color):
    self.size = size   # 인스턴스 변수 생성 및 초기화
    self.color = color # 인스턴스 변수 생성 및 초기화
    Car.instance_count = Car.instance_count + 1  #클래스 변수 이용
    print('자동차 객체의 수: {}'.format(Car.instance_count))

  def move(self):
    print('자동차({} & {})가 움직입니다.'.format(self.size, self.color))
    
car1 = Car('small','white')
car2 = Car('big', 'black')

> 자동차 객체의 수: 1
> 자동차 객체의 수: 2
# 객체를 생성할 때마다 클래스 변수 instance_count가 1씩 증가 => Car 클래스의 객체가 총 몇 개 생성됐는 지 파악 가능



## 이름이 같은 클래스 변수와 인스턴스 변수가 있는 클래스 ##
class Car2():
  count = 0

  def __init__(self, size, num):
    self.size = size
    self.count = num #인스턴스 변수 생성 및 초기화
    Car2.count = Car2.count + 1 #클래스 변수 이용 -> 클래스명.변수명
    print('자동차 객체의 수: Car2.count = {}'.format(Car2.count))
    print('인스턴스 변수 초기화: self.count = {}'.format(self.count))
  
  def move(self):
    print('자동차({}, {})가 움직입니다.'.format(self.size, self.num)

  
  car1 = Car2('big',20)
  car2 = Car2('small',30)

>자동차 객체의 수: Cloud.count = 1
인스턴스 변수 초기화: self.count = 20
자동차 객체의 수: Cloud.count = 2
인스턴스 변수 초기화: self.count = 30
# 클래스 변수 count, 인스턴스 변수 count가 별개로 동작
```

3. 클래스에서 사용하는 함수
   1. 인스턴스 메서드
   - 각 객체에서 개별적으로 동작하는 함수를 만들고자 할 때 사용하는 함수
   - 인스턴스 메서드는 객체를 생성한 후에 호출
   - **self.함수명()**
   - 인스턴스 메서드 안에서는 'self.함수명()' 형식으로 클래스 내의 다른 함수를 호출할 수 있음. 이때 인자에 self는 전달하지 않음
  
  2. 정적 메서드
   - 클래스와 관련이 있어서 클래스 안에 있지만 클래스나 클래스의 인스턴스(객체)와는 무관하게 독립적으로 동작하는 함수를 만들고 싶을 때 이용
   - 함수를 정의할 때 인자로 self를 사용하지 않음
   - 정적 메서드 안에서는 인스턴스 메서드, 인스턴스 변수에 접근 불가
   - @staticmethoe 선언해 정적 메서드임을 표시
   - 객체 생성 이후 정적 메서드를 호출할 수 있지만 보통 객체 생성하지 않고 클래스명을 이용해 바로 메서드 호출 **'클래스명. 메서드명()'**
   - 날짜, 시간, 환율, 단위 변환에 주로 이용
  
  3. 클래스 메서드
   - 클래스 변수를 사용하기 위한 함수
   - 클래스 메서드를 정의할 때 첫번째 인자로 클래스를 넘겨받는 **cls**가 필요
   - @classmethod 선언해 클래스 메서드임을 표시
   - 객체 생성하지 않고 클래스명을 이용해 바로 호출 **'클래스명. 메서드명()'**
   - 생성된 객체의 개수 반환 등 클래스 전체에서 관리해야 할 기능이 있을 때 주로 이용







```python
class Car():
  instance_count = 0 # 클래스 변수

  def __init__(self, size, color):
    self.size = size
    self.color = color
    Car.instance_count += 1
    print('객체의 수: {}'.format(Car.instance_count))

  def move(self, speed):
    self.speed = speed # 인스턴스 변수 생성
    print('자동차({} & {})가'.format(self.size, self.color), end = ' ')
    print('시속 {}킬로미터로 전진'.format(self.speed))

  def auto(self):
    print('자율 주행 모드')
    self.move(self.speed) 
    # 인스턴스 메서드(move()) 호출
    # move() 함수의 인자로 인스턴스 변수 입력
  
  @staticmethod
  def check_type(model_code):
    if model_code >= 20:
      print('이 자동차는 전기차')
    elif 10 <= model_code < 20:
      print('이 자동차는 가솔린차')
    else:
      print('이 자동차는 디젤차')

  @classmethod
  def count_instance(cls):
    print('자동차 객체의 수: {}'.format(cls.instance_count))


car1 = Car('small','red')
car2 = Car('big','green')

> 객체의 수: 1
객체의 수: 2


car1.move(80) 
car2.move(100)
> 자동차(small&red)가 시속 80킬로미터로 전진
자동차(big&blue)가 시속 100킬로미터로 전진


car1.auto()
car2.auto()
> 자율 주행 모드
자동차(small&red)가 시속 80킬로미터로 전진
자율 주행 모드
자동차(big&blue)가 시속 100킬로미터로 전진


Car.check_type(25) #클래스명.정적메서드()로 호출
> 이 자동차는 전기차

Car.count_instance() #클래스 메서드 호출
> 자동차 객체의 수: 2
```


4. 상속

```python
class FoldingBicycle(Bicycle):
  def __init__(self, wheel_size, color, state):
    Bicycle.__init__(self, wheel_size, color) #Bicycle의 초기화 재사용
    # super.__init__(wheel_size, color) 이용 가능
    self.state = state

  def fold(self):
    self.state = 'folding'
    print('자전거: 접기, state = {}'.format(self.state))
  
  def unfold(self):
    self.state = 'unfolding'
    print('자전거: 접기, state = {}'.format(self.state))

fold_bicycle = FoldingBicycle(25, 'white','unfolding')

fold_bicycle.move(20) # 부모 클래스의 메서드 호출
> 자전거: 시속 20킬로미터로 전진

fold.bicycle.fold()
> 자전거: 접기, state = folding

