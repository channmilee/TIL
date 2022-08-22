### 데이터 시각화

[matplotlib](https://matplotlib.org/)

```python
!apt-get update
!apt-get install -y fonts-nanum
!fc-cache -fv
!rm ~/.cache/matplotlib -rf 
```
```python
import matplotlib.pyplot as plt

# 별도의 팝업창에 그래프 출력
%matplotlib qt

# 그래프를 다시 코드 결과 출력 부분에 출력하려면 다음을 먼저 실행
%matplotlib 

# 새로운 그래프 창 생성
plt.figure()
```

---

#### 선 그래프

 > plt.plot([x,]y[,fmt])
 > plt.show()

```python
1. 여러 그래프 그리기
1)
plt.plot(x, y1)
plt.plot(x, y2)
plt.plot(x, y3)
plt.show

2)
plt.plot(x, y1, x, y2, x, y3)
plt.show()

3)
plt.figure(1)
plt.plot(x, y1) 
plt.figure(2) 
plt.plot(x, y2)
plt.show()

2. 그래프 지우기
plt.figure(2) # 이미 생성된 2번 그래프 창을 지정
plt.clf()     # 2번 그래프 창에 그려진 모든 그래프 지움

3. plt.subplot(m,n,p)
- 하나의 그래프 창을 하위 그래프 영역으로 나눈 후에 각 영역에 그래프를 그리는 방법

plt.subplot(2,2,1) # p는 1 #ax[0][0]
plt.plot(x,y1)

plt.subplot(2,2,2) # p는 2 #ax[0][1]
plt.plot(x,y2)

plt.subplot(2,2,3) # p는 3 #ax[1][0]
plt plot(x y3)

plt.subplot(2,2,4) # p는 4 #ax[1][1]
plt.plot(x,y4)

plt.show()

4. 그래프 출력 범위 지정하기
plt.xlim(xmin, xmax) # x축의 좌표 범위 지정(xmin ~ xmax)
plt.ylim(ymin, ymax) # y축의 좌표 범위 지정(xmin ~ xmax)

5. 
plt.xlabel('X-axis') #x축
plt.ylabel('Y-axis') #y축

plt.title('Graph title')

plt.grid()

plt.legend(['data1','data2'], loc = 4) #범례 #loc는 범례 위치

plt.text(0,6, '문자열 출력1') # 그래프 창에 문자열 표시

6. 한글 입력
import matplotlib as mpl
import matplotlib.pyplot as plt
mpl.rcParams['axes.unicode_minus'] = False
plt.rc('font', family='NanumGothic')
```

---

#### 산점도
- 두 개의 요소로 이뤄진 데이터 집합의 관계
  
 > plt.scatter(x, y [,s=size_n, c=colors, marker='marker_string', alpha=alpha_f])
 > - 0 <= alpha (투명도) <=1

---

#### 막대 그래프
- 여러 항목의 데이터를 서로 비교할 때 주로 이용

 > plt.bar(x, height [,width=width_f, color=colors, tick_label=tick_labels, align=’center'(기본) 혹
은 ’edge’, label=labels])
 > - tick_label: 막대 그래프 각각의 이름 지정
 > - align: 막대 그래프 위치 center/edge
 > - plt.barh() 가로 막대 ,width 옵션은 사용 불가

```python
barWidth = 0.4
plt.bar(index, before_ex, color='c', align='edge', width = barWidth, label = before)
plt.bar(index + barWidth, after_ex , color='m', align='edge', width = barWidth, label = after)

plt.xticks(index + barWidth, member_IDs)
plt.legend()
plt.xlabel('회원 ID')
plt.ylabel('윗몸일으키기 횟수')
plt.title('운동 시작 전과 후의 근지구력(복근) 변화 비교')

plt.show()
```

---

#### 히스토그램
- 데이터를 정해진 간격으로 나눈 후 그 간격 안에 들어간 데이터 개수를 막대로 표시한 그래프
- 변량
- 계급: class 설정 ex. 60~70, 70~80, 80~90, 90~100
- 도수: 각 계급 발생 건수 (bins)
- 도수분포표를 막대그래프로 시각화 표현 = 히스토그램
  
 > plt.hist(x, [bins = bins_n or 'auto'])
 > - x = 변량 데이터
 > - bins = 계급의 갯수로 이 갯수만큼 자동으로 계급이 생성되어 히스토그램을 그림/기본 값은 10

--- 

#### 파이 그래프
 > plt.pie(x, [,labels = label_sep, autopct='비율 표시 형식(ex: %0.1f)', shadow = False(기본) 혹은 True, explode = explode_seq, counterclock = True/False startangle = 각도 (기본은 0) ])
 > - counterclock = True (시계 반대 방향으로), False(시계 방향으로)
 > - plt.figure(figsize = (w,h)) # 가로, 세로 비율이 1대1로 같아야 그래프가 원형으로 보임

 ```python
explode_value = (0.1, 0, 0, 0, 0)

plt.figure(figsize=(5,5))
plt.pie(result, labels= fruit, autopct='%.1f%%',startangle=90, counterclock = False, explode = explode_value, shadow = True)
plt.show()
```
---

#### 그래프 저장하기

> plt.savefig(file_name, [,dpi = dpi_n(기본은 72)])
> - dpi 해상도
> - figure(figsize = (*,h)) 그래프 크기 조절

```python
# 그래프 크기 확인
import matplotlib as mpl
mpl.rcParams['figure.figsize']

# dpi 확인
mpl.rcParams['figure.dpi']

# savefig 한 후 show()해야 함
plt.savefig('saveFigTest1.png', dpi = 100) 
plt.show()
```
---

#### pandas로 그래프 그리기
> Series_data.plot([kind='graph_kind' ][,option])
> DataFrame_data.plot([x=label 혹은 position, y=label 혹은 position,] [kind='graph_kind'] [,option])
> - kind: 그래프 종류 선택, plot.graph_kind() 사용 가능

```python
1.
df_rain = pd.read_csv('sea_rain1.csv', index_col="연도" )

rain_plot = df_rain.plot()
rain_plot.set_xlabel("연도")        # x축 이름 설정
rain_plot.set_ylabel("강수량")      # y축 이름 설정
rain_plot.set_title("연간 강수량")  # 그래프 이름 설정
plt.show()


2.
df_icecream.plot(x = '기온', y = '아이스크림 판매량', grid = True, kind = 'scatter')
plt.show()

df_icecream.plot.scatter(x = '기온', y = '아이스크림 판매량', grid = True)
plt.show()