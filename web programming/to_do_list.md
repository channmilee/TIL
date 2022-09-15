### 1. 프로젝트 뼈대 만들기
   > 1) django-admin startproject ToDolist
   >    - ToDoList 라는 프로젝트 생성
   > 2) python manage.py startapp my_to_do_app
   >    - my_to_do_app이라는 어플리케이션 생성
   >>    ToDoList 파일에서 어플리케이션 생성
   >>    - C:\MyTest > cd ToDoList
   > 3) notepad settings.py
   >    - 설정 파일 확인 및 수정
   >>    ToDoList 폴더의 settings.py 파일 열어서 수정
   >>    - Installed apps ("my_to_do_app")
   >>    - 타임존 지정
   > 4) python manage.py migrate
   >    - 데이터베이스에 기본 테이블 생성
   >    - user 그룹 만들어줌
   >> ToDoList 폴더에서 migrate 실행
   >> - db.sqlite3 생김
   > 5) python manage.py runserver
   >    - 현재까지 작업을 개발용 웹 서버로 확인 (127.0.0.1:8000)
   >>- 계정 만들기
   >> - python manage.py createsuperuser
   >>- 뼈대 확인하기
   >> - C:\MyTest>tree /F ToDoList

---

### 2. 모델
- DB에 테이블 생성 (객체)

> 1) notepad modes.py
> - 테이블 정의
>> ToDo 테이블 생성
>> - Content Varchar(255)
>> - isDone Boolean
```python
# my_to_do_app 폴더의 models.py에서
from dango.db import models

class Todo(models.Model):
    content = models.CharField(max_length = 255)
    isDone = models.BooleanField(default=False) #완료버튼에 사용

```

> 2) notepad admins.py
> - 정의된 테이블이 admin 화면에 보이게 함
```python
# my_to_do_app 폴더의 admin.py에서
from django.contrib import admin
from my_to_do_app.models import Todo


admin.site.register(Todo)
```
> 3) python manage.py makemigraions
> - DB 변경이 필요한 사항을 추출함
> - migrations 디렉토리 하위에 마이그레이션이 생김
> 4) python manage.py migrate
> - 데이터 베이스에 변경 사항 반영
> - migrate 명령으로 DB에 테이블 생성
> 5) python manage.py runserver
> - 현재까지 작업을 개발용 웹 서버로 확인함

---
### 3. URLconf 코딩
1. urls.py(C:\MyTest\ToDoList\ToDoList)
2. urls.py(C:\MyTest\ToDoList\my_to_do_app)
```python
# 1. urls.py(C:\MyTest\ToDoList\ToDoList)
from django.contrib import admin
from django.urls import path, **include**

urlpatterns = [
    path("admin/", admin.site.urls),
	**path('admin/', admin.site.urls),**
]


# 2. urls.py(C:\MyTest\ToDoList\my_to_do_app)
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('createTodo/', views.createTodo, name='createTodo'),
    path('doneTodo/', views.doneTodo, name='doneTodo')
 ]

```
---
### 4. template & view
<index() 및 템플릿 작성>
1. 내용 구현을 위해 index.html 
2. 뷰 코딩을 위해 views.py에 함수 정의

```python
# my_to_do_app > views.py
from django.shortcuts import render
from django.http import HttpResponse, HttpResponseRedirect
from django.urls import reverse
from .models import * # models.py에서 생성된 객체 모두 import(class Todo)

def index(request):
     todos = Todo.objects.all() # Todo 클래스 이용해 db에 저장된 모든 레코드 select
     content = {'todos': todos} #content dict 생성
     return render(request, 'my_to_do_app/index.html', content) #index.html 파일로 전달
     # # return HttpResponse("my_to_do_app first page")
     # return render(request, 'my_to_do_app/index.html')

def createTodo(request):
     user_input_str = request.POST['todoContent']
     new_todo = Todo(content=user_input_str)
     new_todo.save()
     return HttpResponseRedirect(reverse('index'))
     # return HttpResponse("DB에 저장되었어요 =>" + user_input_str)

def doneTodo(request):
    done_todo_id = request.GET['todoNum']
    print("완료한 todo의 id",done_todo_id)
    todo = Todo.objects.get(id = done_todo_id)
    todo.isDone = True
    todo.save()
    return HttpResponseRedirect(reverse('index'))

# def deleteTodo(request):
#     done_todo_id = request.GET['todoNum']
#     print("완료한 todo의 id",done_todo_id)
#     todo = Todo.objects.get(id = done_todo_id)
#     # todo.isDone = True
#     todo.delete()
#     return HttpResponseRedirect(reverse('index'))
```
---
작업 확인하기 -- DB에서 데이터 확인
C:\MyTest\ToDoList> python manage.py dbshell
