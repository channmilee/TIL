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
```