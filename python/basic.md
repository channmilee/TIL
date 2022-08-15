### 진수 표현과 변환

1. 2진수
   - 0b
   - bin(17)
2. 8진수
   - 0o
   - oct(17)
3. 16진수
   - 0x
   - hex(17)

> 출력 결과는 작은 따옴표('')안에 있음
> 
> 출력 결과는 숫자가 아니라 **문자열**


---

### set
```python
1. 교집합
A.intersection(B)
A&B

2. 합집합
A.union(B)
A|B

3. 차집합
A.difference(B)
A-B
```

---

### 딕셔너리
```python
country_capital = {'영국':'런던','프랑스':'파리'}
country_capytal['한국'] = '서울'

country_capital

{'한국':'서울','영국':'런던','프랑스':'파리'}
> 키, 값을 추가할 때 순서대로 출력되지 않음
```

---

### zip() 함수

```python
names= ['mimi','sisi','jiji']
ages = [25,21,28]

for name, age in zip(names, ages):
    print(name, age)
```

---

### break, continue
```python
k = 0
while True:
  k += 1
  if k == 2:
    print('next')
    continue
  if k > 4:
    break
  print(k)

1
next
3
4

> continue : 반복문에서 continue를 만나면 그 이후 코드는 실행하지 않고 반복문의 처음으로 가서 바로 다음 반복을 수행
> break : 특정 조건을 맍고할 때 반복문을 멈춤
```

---

### 리스트 컴프리헨션
```python
number = [1,2,3,4,5]
square = []
for i in numbers:
    if i >= 3:
        square.append(i**2)
print(square)

square = [i**2 for i in numbers if i >= 3]
print(square)
```

---

### 형식 지정 출력
```python
name = '강식'
age = 25
print('%s는 내 친구입니다.\n그녀는 %d살 입니다.' %(name, age))

강식이는 내 친구입니다.
그녀는 25살입니다.
```

