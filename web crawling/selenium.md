### selenium 
> 동적 web 페이지 크롤링
> 웹 자동화

```python
from selenium import webdriver

#1. 브라우저 열기
##1. 직접 드라이버의 경로 지정하여 실행
driver = webdriver.Chrome('C:\Myexam\chromedriver/chromedriver.exe')

##2. 드라이버 파일이 같은 디렉토리에 있는 경우 실행
driver = webdriver.Chrome()


#2. 웹 드라이버 객체 만들기 및 페이지 이동
# driver의 get 함수를 이용하여 크롬 브라우져 페이지로 이동
# url은 http를 포함해야 함
driver.get('http://www.naver.com/')


#3. 윈도우 사이즈 조절(가로, 세로)
driver.set_window_size(1024,768)


#4.브라우저의 스크롤 위치 이동 - 자바 스크립트 이용(가로, 세로)
driver.execute_script('window.scrollTo(200,300);')


#5.Alret 다루기
## alert 체크
try:
    alert = driver.switch_to.alert
    print(alert.text)
except:
    print('alert 없음')

driver.execute_script("alert('selenium test');")

## alert 확인 버튼 누르기
alert.accept()


#6. 버튼 클릭하기
driver.find_element(By.CSS_SELECTOR, 'btn_submit').click()
```

---

##### 테드 홈페이지 접속 & 텍스트 데이터 추출

```python
# 테드 사이트 접속
driver.get('http://www.ted.com/talks')

# 메인 배너 타이틀 가져오기
driver.find_element(By.CSS_SELECTOR, '#banner-secondary').text

# 컨텐츠 리스트 제목 가져오기
contents = driver.find_elements(By.CSS_SELECTOR, '#browse-results > div > .col')

# 셀렉터 확인
contents[0].find_element(By.CSS_SELECTOR, '.media > .media_message .ga-link').text

# 전체 데이터 가져오기
titles = []
for content in contents:
    title = content.find_element(By.CSS_SELECTOR, '.media > .media_message .ga-link').text
    titles.append(title)

# 사용 가능한 언어 옵션 리스트 가져오기
languages = driver.find_element(By.CSS_SELECTOR, '#languages').text
languages = languages.split('\n')[1:-1]
languages

# 한국어 선택하기
driver.find_element(By.CSS_SELECTOR, '#languages [lang='ko']').click()

# 컨텐츠 가져오기
import time
time.sleep(3)
contents = driver.find_elements(By.CSS_SELECTOR, '#browse-results > div > .col')
titles = []
for content in contents:
    title = content.find_element(By.CSS_SELECTOR, '.media > .media_message .ga-link').text
    titles.append(title)
titles[-5:]

# 컨텐츠 링크 가져오기
time.sleep(3)
contents = driver.find_elements(By.CSS_SELECTOR, '#browse-results > div > .col')
links = []
for content in contents:
    link = content.find_element(By.CSS_SELECTOR, '.media > .media_message .ga-link').text
    links.append(title)
links[-5:]

# 브라우져 종료하기
driver.quit()
```

---

##### 네이버 기사 자동 크롤링

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

article_list = []

def get_article(page):
    driver = webdriver.Chrome('C:\Myexam\chromedriver/chromedriver.exe')
    driver.get('http://news.naver.com/main/main.nhn?mode=LSD&mid=shm&sid1=105#&date=%2000:00:00&page=')
    articles = driver.find_elements(By.CSS_SELECTOR, '#section_body li')
    for article in articles:
               title = article.find_element(By.CSS_SELECTOR, 'dt:not(.photo)>a').text
               article_list.append(title)

    print('end:', page)
    
    driver.quit()

%%time

for page in range(1,5):
    get_article(page)

len(article_list), article_list[:30]

# threading -> 동시에 많은 크롤러로 인해 에러 방지 목적
import threading
import pandas as pd

df = pd.DataFrame(columns = ['title'])

def get_article(page):
    driver = webdriver.Chrome('C:\Myexam\chromedriver/chromedriver.exe')
    driver.get('http://news.naver.com/main/main.nhn?mode=LSD&mid=shm&sid1=105#&date=%2000:00:00&page=')
    articles = driver.find_elements(By.CSS_SELECTOR, '#section_body li')
    for article in articles:
        title = article.find_element(By.CSS_SELECTOR, 'dt:not(.photo)>a').text
        df.loc[len(df)] = {
            'title':title,
        }       
    
    driver.quit()
    
%%time
for page in range(1,5):
    get_article(page)
```