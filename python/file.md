데이터 파일 읽고 처리 과정
1. 데이터 파일 읽어오기
   - 파일 형식 파악 (txt, excel, csv 등)
   - 파일 내용 데이터 어떤 형태로 되어 있는지?

2. 데이터 전처리
   - 데이터 처리할 자료형
   - 데이터 원하는 형태로 분리
   - 연산이 필요한 부분 숫자 데이터로 변환

---

### 파일 읽고 열기
```python
f = open('my.txt', 'w')
f.write('hi')
f.close()

f = open('my.txt','r')
file_txt = f.read()
f.close
print(file_txt)
```
---

### 반복문을 이용해 파일 읽고 쓰기

1) 파일에 문자열 한 줄씩 쓰기
```python
f = open('two_times_table.txt', 'w')
for i in range(1,10):
  format_string = '2 x {} = {}\n'.format(i, i*2)
  # write 함수는 자동 줄 바꿈이 불가능. 개행문자(\n)추가해 줄 바꿈
  f.write(format_string)
  # 파일에 문자열 저장
f.close()
```

2) 파일에서 문자열 한 줄씩 읽기
  > readline()
  > - 실행 횟수만큼 문자열을 한 줄씩 읽음
  > - 파일의 마지막 한 줄을 읽고 나서 readline()을 수행하면 빈 문자열 반환
```python
f = open('two_times_table.txt','r')
line1 = f.readline()
line2 = f.readline()
f.close()
print(line1, end = '')
print(line2, end = '')

> 2 x 1 = 2
> 2 x 2 = 4

* while문 이용하기
f = open('two_times_table.txt', 'r')
line = f.readline()
# 문자열 한 줄씩 읽기
while True:
   # line이 공백인지 검사해서 반복 여부 결정
  print(line, end = '')
  line = f.readline()
  # 문자열 한 줄 읽기
  # 마지막 line이 빈 문자열이기 때문에 while문 빠져나옴
f.close()
```

> readlines()
> - 파일 전체의 모든 줄을 읽어서 한 줄씩 요소로 갖는 리스트 타입으로 반환

 ```python
f = open('two_times_table.txt')
lines = f.readlines()
f.close()

print(lines)
> ['2 x 1 = 2\n', '2 x 2 = 4\n', '2 x 3 = 6\n', '2 x 4 = 8\n', '2 x 5 = 10\n', '2 x 6 = 12\n', '2 x 7 = 14\n', '2 x 8 = 16\n', '2 x 9 = 18\n']
# 리스트 형태로 반환

1)
f = open('two_times_table.txt')
lines = f.readlines()
f.close()
for line in lines:
  print(line, end = '')

2)
f = open('two_times_table.txt')
for line in f.readlines():
  print(line, end = '')
f.close()

3)
f = open('two_times_table.txt')
for line in f:
  print(line, end = '')
f.close()

# 1), 2), 3) 같은 결과 출력됨
```

---

### with 
1. 파일 쓰기
   with open('file_name','w') as f : 
      f.write(str)

2. 파일 읽기
   with open('file_name','r') as f : 
      data = f.read()