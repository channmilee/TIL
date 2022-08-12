### 오류 처리 문법
```python
try:
실행한 명령

except  예외 as 변수 :
오류 처리문

else:
예외가 발생하지 않을 때의 처리

finally:
예외 발생 여부와 무관하게 반드시 실행해야 할 명령

# 파일 close()는 주로 이 위치에 있음

```

[예외처리 참고](https://docs.python.org/ko/3/library/exceptions.html)

-----
#### 예시
```python
def division(a,b):

x = a

print(x)

# 1) 오류 발생 x

try:
division(4,2)

except ZeroDivisionError as e:
print('0으로 나누지 마세요. {}에러가 발생합니다.'.format(e))
# ZeroDivisionError 정해진 문법

else:
print('나눗셈이 정상적으로 실행 되었습니다.')

finally:
print('프로그램을 종료합니다.')


결과값: 
2.0
나눗셈이 정상적으로 실행되었습니다. 
프로그램을 종료합니다. 

# 2) 오류 발생 o

try:
division(4,0)

except ZeroDivisionError as e:
print('0으로 나누지 마세요. {}에러가 발생합니다.'.format(e))
# ZeroDivisionError 정해진 문법

else:
print('나눗셈이 정상적으로 실행 되었습니다.')

finally:
print('프로그램을 종료합니다.')

결과값: 
0으로 나누지 마세요. division by zero에러가 발생합니다.
프로그램을 종료합니다.

## 실제 에러에 division by zero라고 나옴
```
