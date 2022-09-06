### 웹 크롤링
#### 수집
1. webbrowser
   ``` python
   # 웹 사이트에 접속하기
   import webbrowser

   url = 'www.naver.com'
   webbrowser.open(url)

   # 네이버에서 특정 검색어 입력해 결과 얻기
   import webbrowser
   naver_search_url = "https://search.naver.com/search.naver?sm=tab_hty.top&where=nexearch&query="
   search_word = "파이썬"
   url = naver_search_url + search_word

   webbrowser.open_new(url)

   # 여러 개의 웹 사이트에 접속하기
   import webbrowser
   urls = ['www.naver.com','www.daum.net', 'www.google.com']for url in urls:
   webbrowser.open_new(url)

   # 여러 단어 리스트와 for문 이용
   import webbrowser

   naver_search_url = "https://search.naver.com/search.naver?sm=tab_hty.top&where=nexearch&query="
   search_words = ['python','java']

   for search_word in search_words:
    webbrowser.open_new(naver_search_url + search_word)
    ```

2. urllib
```python
from urllib.request import urlopen

url = 'http://www.naver.com'
html = urlopen(url)
html

# 응답객체에서 소스코드 읽어오기
# 응답객체.read() --> 한번 읽어오면 그 후에는 읽어도 빈 문자열이 나옴
rawtext = html.read()

type(rawtext)
> bytes
bytes(rawtext)
print(rawtext.decode('utf8'))
```

---

3. request
> requests 패키지의 get(url) 함수 사용 
> urllib의 open(), read()를 한꺼번에 진행
> url 변수에 저장되어 있는 웹주소로 요청신호를 보냄
> response 변수에는 url 변수의 소스코드(html)이 저장되어 있음

```python
import requests

r = requests.get('http://www.google.co.kr')
r
> <Response [200]>

#정상 응답인지 확인
response.status_code

type(r)
> requests.models.Response

type(r.text)
> str

len(r.text)
r.text[0:100]
```

#### 파라미터 전달 방법
> 파라미터란? 
> - 사이트의 문서를 요청할 때 서버로 전달되는 정보
- 문서를 찾기 위한 정보나 명령을 수행하기 위한 정보를 같이 전달하게 되는데 그 정보를 파라미터라고 함
> 서버에 파라미터 전송방법
> param = {'param1':'value','param2':'value2'}
> res = requests.get(url, params = param)
> 
```python
# 기본 url
base_url = 'http://sports.news.com/news'

# 파라미터가 포함된 url
param_url = 'http://sports.news.com/news?oid=477&aid=0000312064'

# 파라미터 전달 방식1 --> url에 파라미터 포함
res1 = requests.get(param_url)
res1.status_code


# 파라미터 전달 방식2 --> params = dict 사용
param = {'oid':477, 'aid':'0000312064'}
res2 = requests.get(base_url,params=param)
res2.status_code
```

### 분석
>> BeautifulSoup 라이브러리를 이용해 HTML 소스 파싱

```python

from bs4 import BeautifulSoup

html = """<html><body><div><span>\
<a href=http://www.naver.com>naver</a>\
<a href=http://www.google.com>google</a>\
<a href=http://www.daum.net>daum</a>\
</span></div></body></html>"""

# 응답 객체인 html을 BeautifulSoup(응답객체, 파서기) 함수에 전달
# bs4 파싱 객체 반환 --> bs_obj 객체 생성됨
soup = BeautifulSoup(html,'lxml')
soup


# 문자열 형태에서 변환
type(html)
> str
type(soup)
> bs4.BeautifulSoup

# 객체 생성
print(type(soup))
> <class 'bs4.BeautifulSoup'>

# 파싱 결과를 보기 좋게 HTML 형태로 바꿔줌
print(soup.prettify())   
```

---
    
### 추출
> 태그나 속성을 통해 원하는 데이터 추출
> find() : 파싱 결과에서 BeautifulSoup.find('태그') 수행하면 HTML 소스 코드에서 해당 태그가 있는 첫 번째 요소를 찾아서 반환
> findAll() : 리스트 형태로 반환하지만 객체로 tag 구성되어 있음
> get_text() : 태그와 속성 제거하고 텍스트 문자열 추출

```python
html_str = """
<html>
    <body>
        <ul class="greet">
            <li>hello</li>
            <li>bye</li>
            <li>welcome</li>
        </ul>
        <ul class="reply">
            <li>ok</li>
            <li>no</li>
            <li>sure</li>
        </ul>
    </body>
</html>
"""

bs_obj = bs4.BeautifulSoup(html_str, 'lxml')

# 속성값으로 찾기
bs_obj.find('ul',{'class':'great'})
bs_obj.findAll('ul')[1]


bs_obj.find('ul').find_all('li')
bs_obj.find_all('ul')[0].find_all('li')
```
```python
html_str = """
<html>
    <body>
        <h1>파이썬 프로그래밍</h1>
        <p>웹 페이지 분석</p><p>크롤링</p><p>파싱</p>        
    </body>
</html>
"""

bs_obj = bs4.BeautifulSoup(html_str, 'lxml')

p1 = bs_obj.find('p')
p1.next_sibling.next_sibling
```
```python
html_str = """
<html>
    <body>
        <ul class="ko">
            <li><a href="https://www.naver.com/">네이버</a></li>
            <li><a href="https://www.daum.net/">다음</a></li>
        </ul>
        <ul class="sns">
            <li><a href="https://www.goole.com/">구글</a></li>
            <li><a href="https://www.facebook.net/">페이스북</a></li>
        </ul>
    </body>
</html>
"""

# 속성명으로 찾기
bs_obj.find_all('a')[0]['href']
> 'https://www.naver.com/'
hrefs = []
for a in bs_obj.find_all('a'):
    print(a['href'])
    hrefs.append(a['href'])


# 데이터 프레임에 넣기
import pandas as pd

pd.DataFrame({'site명':names, 'siteURL' : hrefs})
```

---

##### BeautifulSoup의 다양한 기능
```python
from bs4 import BeautifulSoup

html2 = """
<html>
 <head>
  <title>작품과 작가 모음</title>
 </head>
 <body>
  <h1>책 정보</h1>
  <p id='book_title'>토지</p>
  <p id='author'>박경리</p>
  
  <p id='book_title'>태백산맥</p>
  <p id='author'>조정래</p>
  
  <p id='book_title'>감옥으로부터의 사색</p>
  <p id='author'>신영복</p>
 </body>
</html>
"""

soup2 = BeautifulSoup(html2, 'lxml')
soup2

soup2.title
<title>작품과 작가 모음</title>

soup2.body
soup2.body.h1

# 태그 내의 속성 찾기
soup2.find('p', {'id':'book_title'})
<p id="book_title">토지</p>

# 조건 만족하는 요소 전부 가져오기
soup2.find_all('p',{'id':'book_title'})
[<p id="book_title">토지</p>,
 <p id="book_title">태백산맥</p>,
 <p id="book_title">감옥으로부터의 사색</p>]

 # 책 제목과 작가를 포함한 요소를 각각 추출한 후 텍스트만 뽑기
 title_names = soup2.find_all('p',{'id':'book_title'})
 authors = soup2.find_all('p',{'id':'author'})

for title_name, author in zip(title_names, authors):
    print(title_name.get_text() + '/' + author.get_text())
토지/박경리
태백산맥/조정래
감옥으로부터의 사색/신영복
```

---

### CSS 선택자
>> BeautifulSoup.select()의 인자로 '태그 및 속성'을 단계적으로 입력하면 원하는 요소를 찾을 수 있음

```python
html_str = """
<html>
   <body>
    	<div id="wrap">
        	<div id="mainMenuBox">                	
                <ul>  
                    <li><a href="#">패션잡화</a></li>    
                    <li><a href="#">주방용품</a></li>                     	          
                    <li><a href="#">생활건강</a></li>
                    <li><a href="#">DIY가구</a></li>
                </ul>
            </div>
        	<div>
            	<table>
                	<tr><td><img src="shoes1.jpg"></td>
                    	  <td><img src="shoes2.jpg"></td>
                    	  <td><img src="shoes3.jpg"></td></tr>
                    <tr id="prdName"><td>솔로이스트<br>걸리쉬 리본단화</td>
                    	  <td>맥컬린<br>그레이가보시스트랩 펌프스</td>
                          <td>맥컬린<br>섹슈얼인사이드펌프스</td></tr>
                    <tr id="price"><td>100,000원</td><td>200,000원</td><td>120,000원</td></tr>
                </table>
            </div>
            <div id="out_box">
            	<div class="box">
                	<h4>공지사항</h4>
                    <hr>
                    <a href="#">[배송] : 무표배송 변경 안내 18.10.20</a><br>
                    <a href="#">[전시] : DIY 가구 전시 안내 18.10.31</a><br>
                    <a href="#">[판매] : 11월 특가 상품 안내 18.11.05</a>                               
                </div>
                <div class="box">
                	<h4>커뮤니티</h4>
                    <hr>
                    <a href="#">[레시피] : 살 안찌는 야식 만들기</a><br>
                    <a href="#">[가구] : 헌집 새집 베스트 가구</a><br>
                    <a href="#">[후기] : 배송이 잘못 됐어요 ㅠㅠ</a><br>
                 </div>
            </div>            
        </div>
    </body>
</html>"""

bs_obj = bs4.BeautifulSoup(html_str, 'lxml')

# 같은 결과 나옴
bs_obj.findAll('a')
bs_obj.select('a')

# id 검색하기
bs_obj.select('div #mainMenuBox')

# 자손, 손자 구분
## mainMenuBox(부모) 는 자손으로 Ul을 가지고 있으므로 두 개는 같은 값을 출력함
bs_obj.select('#mainMenuBox ul')
bs_obj.select('#mainMenuBox > ul')

## li는 mainMenuBox의 손자. 공백은 자손, 손자 다 출력하지만 >는 자손만 나옴
print(bs_obj.select('#mainMenuBox li'))
> [<li><a href="#">패션잡화</a></li>, <li><a href="#">주방용품</a></li>, <li><a href="#">생활건강</a></li>, <li><a href="#">DIY가구</a></li>]
bs_obj.select('#mainMenuBox > li')
> []

# > 는 자손만 출력되니까 3개
bs_obj.select('#wrap > div')
len(bs_obj.select('#wrap > div'))
> 3

# 공백은 자손, 손자 다 출력되니까 5개
bs_obj.select('#wrap div')
len(bs_obj.select('#wrap div'))
> 5

bs_obj.select('.box')
len(bs_obj.select('.box'))
> 2

bs_obj.select('.box')[0].select('a')[0]['href']
> #
 ```

 ```python
 %%writefile C:\Myexam\HTML_example_my_site.html


<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>사이트 모음</title>
  </head>
  <body>
    <p id="title"><b>자주 가는 사이트 모음</b></p>
    <p id="contents">이곳은 자주 가는 사이트를 모아둔 곳입니다.</p>
    <a href="http://www.naver.com" class="portal" id="naver">네이버</a> <br>
    <a href="https://www.google.com" class="search" id="google">구글</a> <br>
    <a href="http://www.daum.net" class="portal" id="daum">다음</a> <br>
    <a href="http://www.nl.go.kr" class="government" id="nl">국립중앙도서관</a>
  </body>
</html>


# html 소스 파일은 이미 저장 돼 있으므로 텍스트 파일을 읽어와서 변수 html3에 할당

f = open('C:\Myexam\HTML_example_my_site.html', encoding = 'utf-8')

html3 = f.read()
f.close()

soup3 = BeautifulSoup(html3, 'lxml')

# 클래스 출력하기 --> '태그.class_속성값'
soup3.select('a.portal')
[<a class="portal" href="http://www.naver.com" id="naver">네이버</a>,
 <a class="portal" href="http://www.daum.net" id="daum">다음</a>]


# 같은 값 나옴
soup3.select('html body a')
soup3.select('body a')
soup3.select('html a')
soup3.select('a')
```

