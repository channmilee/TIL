### Numpy
1. 특징
   - 다차원 배열 데이터를 효과적으로 처리
   - 과학 연산을 쉽고 빠르게
```python
import numpy as np

1. 배열 생성하는 법
data1 = [1,2,3,4,5]
a1 = np.array(data1)
a1

a2 = np.array([0.5,1,0.8])
a3 = np.array([1,2,3],[4,5,6],[7,8,9])
# seq_data를 받아 배열 객체 생성

1-1. 난수 배열 생성
r1 = np.random.rand()
r2 = np.random.randint(1, 30)
# 범위에 해당하는 정수 난수 배열 생성

2. 데이터 타입 확인하기
a1.dtype
a2.dtype

2-1. 데이터 타입 변환
a3 = np.array(['1','3.4','5,678'])
num_a3 = a3.astype(float)
# 문자열 원소를 실수 타입으로 변환

3. 범위 지정해 배열 생성
b1 = np.arange(0,10,2)
b2 = np.arange(12).reshape(4,3)
# 4행 3열로 배열 객체 생성
b2.shape
# 행렬 출력됨 ex. (4,3)
b3 = np.linspace(1,10,10)
# 시작과 끝, 데이터 갯수 지정해 배열 생성

4. 특별한 형태의 배열 생성
zero = np.zeros(n)
one = np.ones(n)
np.eye(3)

5. 기본 연산
- 배열의 형태가 같아야 함 = ndarray.shape의 결과가 같아야 함

A = np.arange([0,1,2,3]).reshape(2,2)
B = np.arange([3,2,1,0]).reshape(2,2)

>> sum(), mean(), std(), var(), min(), max(), cumsum(), cumprod()

1) 행렬 곱
A.dot(B)
np.dot(A,B)

2) 전치 행렬
np.transpose(A)
A.transpose()

3) 역행렬
np.linalg.inv(A)

4) 행렬식
np.linalg.det(A)

6. 슬라이싱
a = np.arange(1,10).reshape(3,3)
a[0:2][1:3]
> array([[4,5,6]])
# a[0:2]가 먼저 실행 되고 [1:3]이 실행됨
a[0:2,1:3]
> array([[2,3],[5,6]])