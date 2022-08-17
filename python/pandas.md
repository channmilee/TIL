### pandas

```python
import pandas as pd
```

---
### Series

```python
1. Series를 활용한 데이터 생성
s = pd.Series([10,20,30,40,50])
s
>0    10
1    20
2    30
3    40
4    50
dtype: int64
# index, values로 구분되어 출력됨

print(s1.index)
> RangeIndex(start=0, stop=5, step=1)

print(s1.values)
> [10 20 30 40 50]
# 결과가 Numpy 배열과 형식이 같음

s2 = pd.Series(['a','b','c',1,2,3])
s2
>0    a
1    b
2    c
3    1
4    2
5    3
dtype: object

# Numpy의 경우 배열의 모든 원소 데이터 타입이 같아야 했지만 pandas는 달라도 됨.

1-1. np.nan
import numpy as np
s3 = pd.Series([np.nan, 10, 30])
s3
>0     NaN  # 데이터를 위한 자리(index)는 있지만 실제 값은 없음
1    10.0
2    30.0
dtype: float64

1-2. 딕셔너리를 활용한 Series
s4 = pd.Series({'국어':100, '영어': 90})
s4
>국어    100
영어     90
dtype: int64
# 딕셔너리의 key -> index / 딕셔너리의 values -> values

2. 날짜 자동 생성: date_range
pd.date_range(start = None, end = None, periods = None, freq = 'D')
# periods = 날짜 데이터 생성 기간
# freq = 날짜 데이터 생성 주기

pd.date_range(start = '2019-01-01', periods = 4, freq = '2D')
> DatetimeIndex(['2019-01-01', '2019-01-03', '2019-01-05', '2019-01-07'], dtype='datetime64[ns]', freq='2D')
```

---
### DataFrame
> Series를 활용해 1차원 데이터 생성

```python
df = pd.DataFrame(data, index = index_data, columns = columns_data)
# data는 리스트, 딕셔너리, Numpy 배열 데이터, Series, DataFrame 타입의 데이터 모두 가능
```

1. DataFrame 형태 
```python
data = np.array([[1,2,3],[4,5,6],[7,8,9],[10,11,12]])
index_date = pd.date_range('2019-09-01', periods = 4)
columns_list  = ['A','B','C']
pd.DataFrame(data, index = index_date, columns = columns_list)

---

table_data = {'연도':[2015,2016,2016,2017,2017],
              '지사':['한국','한국','미국','한국','미국'],
              '고객 수':[200,250,450,300,500]}
df = pd.DataFrame(table_data, columns = ['연도','지사','고객 수'])
df

df.index
> RangeIndex(start=0, stop=5, step=1)

df.columns
> Index(['연도', '지사', '고객 수'], dtype='object')

df.values
> array([[2015, '한국', 200],
       [2016, '한국', 250],
       [2016, '미국', 450],
       [2017, '한국', 300],
       [2017, '미국', 500]], dtype=object)


2. 데이터 연산
- Series()와 DataFrame()으로 생성한 데이터끼리는 사칙 연산 가능
- Numpy와 달리 크기가 달라도 연산 가능

df.mean(axis = 1)
# 행 방향 연산
df.mean(axis = 0)
# 열 방향 연산

3. 데이터 원하는 대로 선택하기
df_data.loc[index_name][column_name]
df_data.loc[index_name, column_name]
df_data[column_name][index_name]
df_data[column_name][index_pos]
df_data[column_name].loc[index_name]

4.df_data.T
# 데이터의 행,열을 바꿔줌(전치)

5. 데이터 통합하기
# 데이터가 없으면 NaN으로 채워짐
1) 세로 방향 통합: 'append()'
-> index 증가
1-1) ignore_index = True
# index 번호를 순서대로 지정 

2) 가로 방향 통합: 'join()'
-> columns 증가

3) 특정 열을 기준으로 통합: 'merge()'
-> 특정 열이 같아야 함
-> df_left.merge(df_right, how = 'merge_method', on = 'key_label')
# on 인자는 통합 기준이 되는 특정 열의 라벨 이름을 입력
# how 인자는 지정된 특정 열을 기준으로 통합 방법 지정

6. 파일 읽기
df_data = pd.read_csv('file_name', option)
# option은 구분자 설정 가능 ex. sep = ' '

1) 파일이 한글일 경우
df_data = pd.read_csv('file_name', encoding = 'cp949')

2) index 지정하기
df_data = pd.read_csv('file_name', index_col = 'index_name')

2-1) index 이름 지정하기
df_data.index.name = 'index_name'

3) 파일 저장하기
df_data.to_csv('file_name', option)

4) 열 추가하기
df_data['col_name'] = col_data