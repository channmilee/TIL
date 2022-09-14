### MVC 패턴
- 웹 서버 구현에 사용되는 프레임 워크
- Model, View, Controller을 구부내서 한 요소가 다른 요소들에 영향을 주지 않도록 설계하는 방식
  1. Model: 데이터
  2. View: 사용자 인터페이스
  3. Controller: 데이터를 처리하는 로직

### MVT 패턴
#### <MVT의 일반적인 특징>
1. 장고는 MVC를 기반으로 한 프레임 워크
    > - **Model**: DB에 저장되는 데이터를 의미
    > - **View**: 실질적으로 프로그램 로직이 동작하여 데이터를 가져오고 적절하게 처리한 결과를 템플릿에 전달하는 역할 수행
    > - **Template**: 사용자에게 보여지는 UI 부분
2. ORM(Object-Relational Mapping, 객체 관계 매핑)
   - 데이터 베이스 시스템과 데이터 모델 클래스를 연결시키는 다리 같은 역할
3. 자동으로 구성되는 관리자 화면
   - 장고는 웹 서버의 콘텐츠, 즉 데이터베이스에 대한 관리 기능을 위하여 프로젝트를 시작하는 시점에 기본 기능으로 관리자 화면 제공
4. 우아한 URL 설계
   - URL을 직관적이고 쉽게 표현
5. 자체 템플릿 시스템
   - 장고는 내부적으로 확장이 가능하고 디자인이 쉬운 강력한 템플릿 시스템을 갖고 있음
    - 반복문을 통해 관련 내용을 쉽게 볼 수 있음 -> 확장 가능
    - 디자인과 로직에 대한 코딩을 분리하여 독립적으로 개발 진행
6. 캐시 시스템
   - 동적인 페이지를 만들기 위해서 데이터베이스 쿼리를 수행하고 템플릿을 해석하며, 관련 로직을 실행해서 페이지를 생성하는 일은 서버에 엄청난 부하를 주는 작업
7. 다국어 지원
8. 소스 변경사항 자동 반영
   - 장고에서는 *.py 파일의 변경 여부를 감시하고 있다가 변경이 되면 실행 파일에 변경 내역을 바로 반영

### <장고에서의 애플리케이션 개발 방식>
- 웹 사이트 전체 프로그램 또는 모듈화된 단위 프로그램을 애플리케이션이라고 함
- 즉, 프로그램으로 코딩할 대상을 애플리케이션
- 사이트에 대한 **전체 프로그램**을 프로젝트라고 함
- 모듈화된 **단위 프로그램**을 애플리케이션이라고 함

### <장고 MVT 패턴 순서>
1. 클라이언트로부터 요청을 받으면 URL.conf를 이용하여 URL 분석
2. URL 분석 결과를 통해 해당 URL에 대한 처리를 담당할 **뷰**를 결정
3. 뷰는 자신의 로직을 실행하면서, DB 처리가 필요하면 **모델**을 통해 처리하고 그 결과를 반환
4. 뷰는 자신의 로직 처리가 끝나면 **템플릿**을 사용하여 클라이언트에 전송할 HTML 파일을 생성
5. 뷰는 최종 결과로 HTML 파일을 클라이언트에게 보내 응답

---

#### **1. Model** --> 데이터 베이스 정의(models.py)
- 모델이란 사용될 데이터에 대한 정의를 담고 있는 장고의 클래스
- 장고는 ORM 기법을 사용하여 애플리케이션에서 사용할 DB를 클래스로 매핑해서 코딩할 수 있음

#### **2. URL.conf** --> URL 정의(urls.py)
- 파이썬의 URL 정의 방식은 직관적이고 이해하기 쉬움 --> Elegant URL
- URL 정의하기 위해서는 urls.py 파일에 URL과 처리 함수(view)르를 매핑하는 파이썬 코드 작성하면 됨
- URL 패턴: <type:name> 형식 사용

    > **<웹 클라이언트가 웹 서버에 페이지 요청 시 장고에서 URL 분석하는 순서>**
    > 1. Setting.py 파일의 ROOT_URLCONF 항목을 읽어 최상의 URLconf(urls.py)의 위치 파악
    > 2. URLconf를 로딩하여 urlpatterns 변수에 지정되어 있는 URL 리슽 검사
    > 3. 위에서부터 순서대로 URL 리스트의 내용을 검사하면서 URL 패턴이 매치되면 검사 종료
    > 4. 매치된 URL의 뷰(함수 또는 클래스의 메소드)를 호출
    > 5. 호출 시 HTTPRequest 객체와 매칭할 때 추출된 단어들을 뷰에 인자로 넘겨줌
    > 6. URL 리스트 끝까지 검사했는데도 매칭에 실패하면 에러를 처리하는 뷰를 호출

#### **3. View** --> 로직 정의(view.py)
- 뷰는 웹 요청을 받아서 DB 접속 등 해당 애플리케이션의 로직에 맞는 처리를 함
- 그 결과 데이터를 HTML로 변환하기 위하여 템플릿 처리를 한 후, 최종 HTML로 된 응답 데이터를 웹 클라이언트로 반환

#### **4. Template** --> 화면 UI 정의(*.html)
- 개발자가 응답에 사용할 *html 파일을 작성하면, 장고는 이를 해석해서 최종 HTMl 텍스트 응답을 생성하고, 이를 클라이언트에게 보내줌
- 클라이언트(웹 브라우저)는 응답으로 받는 HTMl 텍스트를 해석해서 우리가 보는 웹 브라우저 화면에 UI를 보여줌
- 템플릿 파일은 *.html 확장자를 가지며, 장고 템플릿 시스템 문법에 맞게 작성
- 장고세어 템플릿 파일을 찾을 때는 TEMPLATES 및 INSTALLED_APPS에서 지정된 디렉토리를 검색
- 이 항목들은 프로젝트 설정 파일인 settings.py 파일에 정의되어 있음

---

### <MVT 코딩 순서>
1. 프로젝트 뼈대 만들기: 프로젝트 및 앱 개발에 필요한 디렉토리와 파일 생성
2. 모델 코딩하기: 테이블 관련 사항을 개발(models.py, admin.py 파일)
3. URLconf 코딩하기: URL 및 뷰 매핑 관계 정의(urls.py 파일)
4. 템플릿 코딩하기: 화면 UI 개발(templates/디렉토리 하위의 *.html 파일)
5. 뷰 코딩하기: 애플리케이션 로직 개발(view.py 파일)


#### 1. 프로젝트 뼈대 만들기 순서
   1.  django-admin startproject mysite 
        -> mysite라는 프로젝트 생성
   2.  python manage.py startapp polls
        -> polls라는 애플리케이션 생성
   3.  notepad settings.py 
        -> 설정 파일을 확인 및 수정

        <프로젝트 설정 파일 변경>
        * ALLOWED_HOSTS 항목 지정 -> ['192.168.35.61','localhost','127.0.0.1']
        * 애플리케이션들은 모두 설정 파일에 등록 -> INSTALLED_APPS = [~, 'polls.apps.PollsConfig',]
        * 프로젝트에 사용할 DB 엔진 -> 장고는 디폴트로 SQLite3 엔진 사용
        * 타임존 지정 -> TIME_ZONE = 'Asia/Seoul'
  
   4.  python manage.py migrate
        -> DB에 기본 테이블을 생성
   5.  python manage.py runserver
        -> 현재까지 작업을 개발용 웹 서버 확인
        
        <작업 확인하기>
        * http://127.0.0.1:8000/admin 주소 입력

        <Admin 사이트 로그인>
        * (base) C:\MyTest\projectsite>python manage.py createsuperuser
  
        <뼈대 확인하기>
        * (base) C:\MyTest>tree /F projectsite

---

#### 2. Model 코딩
- 모델 작업은 데이터베이스에 테이블을 만드는 작업

**1. notepade models.py**
   - 테이블 정의(클래스 생성)
  
**2. notepad admins.py**
   - 정의된 테이블이 Admin화면에 보이게 함(테이블 설정)
  
<DB 변경사항 반영>

**3. python manage.py makemigrations**
   - 데이터베이스에 변경이 필요한 사항 추출
   - 테이블의 신규 생성, 정의 변경 등 DB에 변경이 필요한 사항이 있으면 이를 DB에 실제로 반영해주는 작업

**4. python manage.py migrate**
   - 데이터 베이스에 변경사항 반영(적용)
  
<작업 확인>

**5. python manage.py runserver**
   - 현재까지 작업을 개발용 웹 서버로 확인함
   - runserver 용으로 별도의 cmd 창 여는 것이 편리

---

```python
## 1) models.py --> 클래스로 테이블 정의
from django.db import models


class Question(models.Model): # Question 테이블 만들기
    question_text = models.CharField(max_length=200)   # Question 테이블의 필드 1
    pub_date = models.DateTimeField('date published')  # Question 테이블의 필드 2

    def __str__(self):
        return self.question_text


class Choice(models.Model):  # Choice 테이블 만들기
    question = models.ForeignKey(Question, on_delete=models.CASCADE)   # Choice 테이블의 필드 1, FK 지정 
    # Qustion 테이블과 연결되어 있기 때문에 on_delete = models (같이 지우는 명령)
    choice_text = models.CharField(max_length=200)                     # Choice 테이블의 필드 2
    votes = models.IntegerField(default=0)                             # Choice 테이블의 필드 3

    def __str__(self):
        return self.choice_text
```
---
####
```python
## 2) admin.py --> models.py에서 정의한 테이블을 admin에서 보이도록 등록
from django.contrib import admin
from polls.models import Question, Choice

admin.site.register(Question)
admin.site.register(Choice)

### models.py에서 정의한 Qustion, Choice 클래스 임포트
### admin.site.register() 함수 사용해 임포트한 클래스를 admin에 등록
```

---

#### 3. URLconf 코딩
**1. urls.py 작성**
-   URLconf 내용 코딩

**2. views.index() 함수 작성**
   - index.html 템플릿도 같이 작성
  
**3. views.detail() 함수 작성**
   - detail.html 템플릿도 같이 작성
  
**4. views.vote() 함수 작성**
   - 리디렉션 처리 들어있음

**5. views.results() 함수 작성**
   - results.html 템플릿도 같이 작성


#### 1) urls.py 작성
```python
## 1) 전체 urls.py(C:\MyTest\projectsite\mysite) 파일에 코딩
   urlpatterns = [
    path("admin/", admin.site.urls),
    # 추가
	path('polls/',include('polls.urls')),
    ]

## 2) 애플리케이션 ulrs.py(C:\MyTest\projectsite\mysite\polls) 파일에 코딩
   from django.urls import path
   from polls import views

    app_name = 'polls'
    urlpatterns = [
        path('', views.index, name='index'),      # /polls/
        path('<int:question_id>/', views.detail, name='detail'),       # /polls/5/
        path('<int:question_id>/results/', views.results, name='results'),     # /polls/5/results/
        path('<int:question_id>/vote/', views.vote, name='vote'),      # /polls/5/vote/
    ]
```

#### 2) view.index()
```python
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}


<ul>
    <li><a href = "polls/1/">What is your hobby?</a></li>
    <li><a href = "polls/2/">Who do you like best?</a></li>
    <li><a href = "polls/3/">Where do you live?</a></li>
</ul>
```

#### 3) view.detail() --> detail.html
```python
## detail.html
<h1>{{ question.question_text }}</h1>

{% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
{% csrf_token %}
{% for choice in question.choice_set.all %}
    <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}" />
    <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br />
{% endfor %}
<input type="submit" value="Vote" />
</form>

## polls\view.py에 detail() 함수 작성
def datail(request, question_id):
    question = get_object_or404(Question, pk = question_id)
    return render(request, 'polls/detail.html',{'question':question})

```

#### 4) views.vote()
- vote() 뷰 함수의 호출과 연계된 url은 detail.html 템플릿 파일에서 받음
- vote 버튼을 누르면 vote() 뷰 함수가 호출됨
- views.py 파일에 vote() 뷰 함수 내용 입력

```python
## vote() 뷰 함수
def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice.",
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

#### 5) views.results() 및 리다이랙션 작성
- results() 뷰 함수의 호출과 연계된 url은 votes() 뷰 함수의 리다이렉트 결과
- 폼 데이터를 처리한 후에 그 결과를 보여주는 페이지로 리다이렉트 시켜주기 위해 votes() 뷰 함수에 실행
- views.py 파일을 열고 results() 뷰 함수의 내용 추가

```python
## results() 뷰 함수
def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', {'question': question})
```
- 템플릿은 투표 결과로 각 질문마다 투표 카운트를 보여주는 화면
  --> results.html
