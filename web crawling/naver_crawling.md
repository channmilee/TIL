```python
from urllib.request import urlopen
import bs4

url = 'http://www.naver.com'
html = urlopen(url)
bs_obj = bs4.BeautifulSoup(html, 'lxml')

ul_test = bs_obj.select('#NM_FAVORITE > div.group_nav > ul.list_nav.type_fix')
ul_test


lis = bs_obj.findAll('li',{'class':'nav_item'})

menu = []
url = []

for li in lis:
    a_tag = li.find('a')
    menu.append(a_tag.text)
    url.append(a_tag['href'])    


## 데이터 프레임에 넣기
import pandas as pd

naver_menu = pd.DataFrame({'menu':menu, 'url':url})
naver_menu

# 네이버 뉴스 크롤링
url = 'https://news.naver.com'
html = urlopen(url)
bs_obj = bs4.BeautifulSoup(html,'lxml')

## 뉴스 타이틀 추출
news_title = bs_obj.findAll('div',{'class':'cjs_t'})
news_title
for a in news_title:
    print(a.text)

## 뉴스 내용 추출
news_dec = bs_obj.findAll('p',{'class':'cjs_d'})
news_dec
for b in news_dec:
    print(b.text)


# 네이버 뉴스 섹션 메뉴와 섹션별 url 추출
lis = bs_obj.findAll('li',{'class':'Nlist_item'})
len(lis)

for li in lis:
    a_tag = li.find('a')
    print(a_tag.text,':',a_tag['href'])


## 데이터 프레임에 넣기
section = []
link = []

for li in lis:
    a_tag = li.find('a')
    section.append(a_tag.text)
    link.append(a_tag['href'])

naver_sec = pd.DataFrame({'section':section,'link':link})
naver_sec
```