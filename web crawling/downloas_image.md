```python
import requests

url = 'http://www.python.org/static/img/python-logo.png'
html_image = requests.get(url)
html_image
> <Response [200]>

import os

image_file_name = os.path.basename(url)
image_file_name
> 'python-logo.png'

folder = 'C:\Myexam/download'

if not os.path.exists(folder):
    os.makedirs(folder)

image_path = os.path.join(folder, image_file_name)
image_path
> 'C:\\Myexam/download\\python-logo.png'

imageFile = open(image_path, 'wb')
chunk_size = 1000000

for chunk in html_image.iter_content(chunk_size):
    imageFile.write(chunk)
imageFile.close()

os.listdir(folder)
> ['python-logo.png']


## 이미지 주소를 알 경우
import requests
import os

url = 'http://www.python.org/static/img/python-logo.png'
html_image = requests.get(url)
image_file_name = os.path.basename(url)

folder = 'C:\Myexam/download'

if not os.path.exists(folder):
    os.makedirs(folder)

image_path = os.path.join(folder, image_file_name)

imageFile = open(image_path, 'wb')

chunk_size = 1000000

for chunk in html_image.iter_content(chunk_size):
    imageFile.write(chunk)
imageFile.close()


## 여러 이미지 다운 받기
import requests
from bs4 import BeautifulSoup

url = 'http://reshot.com/search/animal'

html_reshot_image = requests.get(url).text
soup_reshot_image = BeautifulSoup(html_reshot_image, 'lxml')
reshot_image_elements = soup_reshot_image.select('a img')
reshot_image_elements[0:4]

reshot_image_url = reshot_image_elements[1].get('src')


html_image = requests.get(reshot_image_url)

folder = 'C:/Myexam/download'

imageFile = open(os.path.join(folder, os.path.basename(reshot_image_url)),'wb')

chunk_size = 1000000
for chunk in html_image.iter_content(chunk_size):
    imageFile.write(chunk)
imageFile.close()