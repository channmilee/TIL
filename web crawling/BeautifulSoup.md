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
3. request
```python
   import requests

   r = requests.get('http://www.google.co.kr')
   r
   > <Response [200]>

   type(r)
   > requests.models.Response

   type(r.text)
   > str

   html = requests.get('http://google.co.kr').text
   html[0:100]
   > '<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="ko"><head><meta content'
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
    soup = BeautifulSoup(html,'lxml')
    soup

    > <html><body><div><span> <a href="http://www.naver.com">naver</a> <a href="http://www.google.com">google</a> <a href="http://www.daum.net">daum</a> </span></div></body></html>

    # 문자열 형태에서 변환
    type(html)
    > str

    type(soup)
    > bs4.BeautifulSoup

    # 파싱 결과를 보기 좋게 HTML 형태로 바꿔줌
    print(soup.prettify())   

    <html>
    <body>
    <div>
    <span>
        <a href="http://www.naver.com">
        naver
        </a>
        <a href="http://www.google.com">
        google
        </a>
        <a href="http://www.daum.net">
        daum
        </a>
    </span>
    </div>
    </body>
    </html>
```
### 추출
>> 태그나 속성을 통해 원하는 데이터 추출
>> 파싱 결과에서 BeautifulSoup.find('태그') 수행하면 HTML 소스 코드에서 해당 태그가 있는 첫 번째 요소를 찾아서 반환

```python

soup.find('a')
> <a href="http://www.naver.com">naver</a>

# get_text() -- 태그와 속성 제거하고 텍스트 문자열 추출
soup.find('a').get_text()
> 'naver'

# find_all() -- 결과를 리스트 형태로 반환
soup.find_all('a')
> [<a href="http://www.naver.com">naver</a>,
 <a href="http://www.google.com">google</a>,
 <a href="http://www.daum.net">daum</a>]

 site_names = soup.find_all('a')
 for site_name in site_names:
    print(site_name.get_text())
> naver
> google
> daum
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
soup2.select('body h1')
[<h1>책 정보</h1>]

soup2.select('body p')
soup2.select('p')

# id 출력하기 --> 'p#id_속성값'
soup2.select('p#book_title')
[<p id="book_title">토지</p>,
 <p id="book_title">태백산맥</p>,
 <p id="book_title">감옥으로부터의 사색</p>]
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

soup3.select('html body a')
soup3.select('body a')
soup3.select('html a')
soup3.select('a')
```

---

#### 줄 바꿈으로 가독성 높이기
```python
%%writefile C:\Myexam\br_example_constitution.html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>줄 바꿈 테스트 예제</title>
  </head>
  <body>
  <p id="title"><b>대한민국헌법</b></p>
  <p id="content">제1조 <br/>①대한민국은 민주공화국이다.<br/>②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다.</p>
  <p id="content">제2조 <br/>①대한민국의 국민이 되는 요건은 법률로 정한다.<br/>②국가는 법률이 정하는 바에 의하여 재외국민을 보호할 의무를 진다.</p>
  </body>
</html>


# html 파일 읽고 변수(html_source)에 할당한 후 텍스트 추출, 출력하기
from bs4 import BeautifulSoup

f = open('C:/Myexam/br_example_constitution.html', encoding = 'utf-8')
html_source = f.read()
f.close()
soup = BeautifulSoup(html_source, 'lxml')


title = soup.find('p', {'id':'title'})
contents = soup.find_all('p', {'id':'content'})

print(title.get_text())
for content in contents:
    print(content.get_text())

> 대한민국헌법
제1조 ①대한민국은 민주공화국이다.②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다.
제2조 ①대한민국의 국민이 되는 요건은 법률로 정한다.②국가는 법률이 정하는 바에 의하여 재외국민을 보호할 의무를 진다.


# 추출된 HTML 코드에서 줄 바꿈 태그(<br/>)을 개행문자로 바꾸기

html1 = ' <p id="content">제1조 <br/>①대한민국은 민주공화국이다.<br/>②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다.</p><p id="content">제2조 <br/>①대한민국의 국민이 되는 요건은 법률로 정한다.<br/>②국가는 법률이 정하는 바에 의하여 재외국민을 보호할 의무를 진다.</p>'

soup1 = BeautifulSoup(html1, 'lxml')


print('==> 태그 p로 찾은 요소')
content1 = soup1.find('p', {"id":"content"})
print(content1)

br_content = content1.find('br')
print('==> 결과에서 태그 br로 찾은 요소:', br_content)

br_content.replace_with('\n')
print('==> 태그 br을 개행문자로 바꾼 결과')
print(content1)

==> 태그 p로 찾은 요소
<p id="content">제1조 <br/>①대한민국은 민주공화국이다.<br/>②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다.</p>
==> 결과에서 태그 br로 찾은 요소: <br/>
==> 태그 br을 개행문자로 바꾼 결과
<p id="content">제1조 
①대한민국은 민주공화국이다.<br/>②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다.</p>


# 추출된 요소 전체에 적용
soup2 = BeautifulSoup(html1, 'lxml')
content2 = soup2.find('p',{'id':'content'})

br_contents = content2.find_all('br')
for br_content in br_contents:
    br_content.replace_with('\n')
print(content2)

<p id="content">제1조 
①대한민국은 민주공화국이다.
②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다.</p>

# 함수 사용
def replace_newlines(soup_html):
    br_to_newlines = soup_html.find_all('br')
    for br_to_newline in br_to_newlines:
        br_to_newline.replace_with('\n')
    return soup_html

soup2 = BeautifulSoup(html, 'lxml')
content2 = soup2.find('p',{'id':'content'})
content3 = replace_newlines(content2)
print(content3.get_text())

제1조 
①대한민국은 민주공화국이다.
②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다.

# HTML 소스코드를 할당한 변수 html_source에 적용
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')

title = soup.find('p',{'id':'title'})
contents = soup.find_all('p',{'id':'content'})

print(title.get_text())

for content in contents:
    content1 = replace_with(content)
    print(content.get_text(), '\n')

대한민국헌법 

제1조 
①대한민국은 민주공화국이다.
②대한민국의 주권은 국민에게 있고, 모든 권력은 국민으로부터 나온다. 

제2조 
①대한민국의 국민이 되는 요건은 법률로 정한다.
②국가는 법률이 정하는 바에 의하여 재외국민을 보호할 의무를 진다. 