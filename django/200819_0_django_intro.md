[TOC]

# 00_django_intro

> 2020.08.14

## Intro

> 1. python version 3.7 확인
> 2. vscode django extension 설치 및 설정

**설치**

```bash
$ pip install django
```

- 특정 버전 설치

  ```bash
  $ pip install django==2.1.15
  ```

- 설치 확인

  ```bash
  $ pip list
  # python -m django --version
  ```



**프로젝트 생성**

> project 를 생성할 때, Python 이나 Django 에서 사용중인 이름은 피해야 한다. 
>
> `-` 도 사용할 수 없다. (ex. django, test, class, django-test...)

```bash
$ django-admin startproject first_project
```



**서버 실행**

```bash
$ python manage.py runserver
```



**프로젝트 구조**

- `__init__.py`
  - 빈 파일
  - Python에게 이 디렉토리를 하나의 Python 패키지로 다루도록 지시
- `settings.py`
  - 웹사이트의 모든 설정을 포함
  - 우리가 만드는 어떤 application이라도 등록이 되는 곳이며, static files의 위치, database 세부 설정 등이 작성
- `urls.py`
  - 사이트의 url와 view의 연결을 지정
- `wsgi.py`
  - Web Server Gateway Interface
  - 장고 어플리케이션이 웹서버와 연결 및 소통하는 것을 도움
- `asgi.py`
  - new in 3.0
  - Asynchronous Server Gateway Interface
  - 장고 어플리케이션이 비동기식 웹 서버와 연결 및 소통하는 것을 도움



**Application (app)**

- 실제로 어떠한 역할을 해주는 친구가 app.
- 프로젝트는 이러한 어플리케이션의 집합이고, 실제 요청을 처리하고 페이지를 보여주고 하는 것들은 이 어플리케이션의 역할.
- 하나의 프로젝트는 여러 개의 app을 가질 수 있다.
  - app은 하나의 역할 및 기능 단위로 쪼개는 것이 일반적
  - 그러나 작은 규모의 서비스에서는 잘 나누지 않는다. 
  - 반드시 이렇게 나눠야 한다 같은 기준 또한 없다.
- **일반적으로 app 이름은 `복수형`으로 하는 것이 좋다.**



**Application 생성**

```bash
$ python manage.py startapp articles
```



**Application 구조**

- `admin.py`
  - 관리자용 페이지 관련 기능을 작성 하는 곳.
- `apps.py`
  - 앱의 정보가 있는 곳. 
  - 우리는 수정할 일이 없다.
- `models.py`
  - 앱에서 사용하는 Model(Database)를 정의하는 곳.
- `tests.py`
  - 테스트 코드를 작성하는 곳.
- `views.py`
  - view가 정의 되는 곳. 



**Application 등록**

> 반드시 **app 생성 후 등록** 순서를 지켜야한다.

- 방금 생성한 application을 사용하려면 장고 프로젝트에 등록을 해야 한다.

  ```python
  # settings.py
  
  INSTALLED_APPS = [
  		'articles',
      'django.contrib.admin',
      'django.contrib.auth',
      'django.contrib.contenttypes',
      'django.contrib.sessions',
      'django.contrib.messages',
      'django.contrib.staticfiles',
  ]
  ```

  - INSTALLED_APPS의 app order

    ```python
    INSTALLED_APPS = [
        # Local apps
        'blogs.apps.BlogsConfig',
    
        # Third party apps
        'haystack',
    
        # Django apps
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.sites',
    ]
    ```



**Internationalization**

> https://docs.djangoproject.com/en/3.1/topics/i18n/

```python
# settings.py

LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'
```



**runserver** Automatic reloading

- 개발 서버는 요청이 들어올 때마다(코드가 저장될 때 마다) 자동으로 Python 코드를 다시 불러온다. 
- 코드의 변경사항을 반영하기 위해서 굳이 서버를 재가동 하지 않아도 된다. 
- 그러나, 파일을 추가하는 등의 몇몇의 동작(커스텀 필터, 새로운 모듈 추가 등)은 개발 서버가 자동으로 인식하지 못하기 때문에, 이런 상황에서는 서버를 재가동 해야 적용되는 경우도 있다.





---



## Url & Template

**urls.py**

- 장고 서버로 요청(request)이 들어오면, 그 요청이 어디로 가야하는지 인식하고 관련된 함수(view)로 넘겨준다.

- `views.py` 에서 만든 함수를 연결시켜준다.

  ```python
  # first_project/urls.py
  
  from django.contrib import admin
  from django.urls import path
  from articles import views
  
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('index/', views.index),
  ]
  ```

  

**views.py**

```python
# articles/views.py

def index(request): # 첫번째 인자는 반드시 request
		return render(request, 'index.html') # render의 첫번째 인자도 반드시 request
```



**Templates**

- `views.py`에서 지정한 `index.html` 파일을 만들자.

- Django에서 template이라고 부르는 HTML 파일은 기본적으로 **app 폴더안의 templates 폴더 안에 위치**한다. 

  ```html
  <!-- articles/templates/index.html -->
  
  <h1>만나서 반갑습니다!</h1>
  ```



---



## Template

> https://docs.djangoproject.com/ko/3.1/topics/templates/#context



### Template Variable

- `render()`를 사용하여 views.py에서 정의한 변수를 template 파일로 넘겨 사용하는 것.

- `render()`의 세번째 인자로 `{'key': value}` 와 같이 딕셔너리 형태로 넘겨주며, 여기서 정의한 `key`에 해당하는 문자열이 template에서 사용 가능한 변수명이 된다.

  ```python
  # articles/views.py
  
  def dinner(request):
      menus = ['족발', '햄버거', '치킨', '초밥']
      pick = random.choice(menus)
      context = {
          'pick': pick,
      }
      return render(request, 'dinner.html', context)
  ```

  ```html
  <!-- articles/templates/dinner.html -->
  
  <h1>오늘 저녁은 {{ pick }}!</h1>
  ```

  

**django imports style guide**

> https://docs.djangoproject.com/en/3.1/internals/contributing/writing-code/coding-style/#imports

```python
# standard library
import json

# third-party
import bcrypt

# Django
from django.http import Http404

# local Django
from .models import LogEntry
```



### Variable Routing

> 주소 자체를 변수처럼 사용해서 동적으로 주소를 만드는 것
>
> https://docs.djangoproject.com/en/3.1/topics/http/urls/#path-converters

```python
# first_project/urls.py

urlpatterns = [
    ... ,
    # 혹은 path('hello/<name>/', views.hello),
    path('hello/<str:name>/', views.hello),
]
```

- default 는 `str` 이기 때문에 생략 가능하다.

```python
# articles/views.py

def hello(request, name):
    context = {
        'name': name,
    }
    return render(request, 'hello.html', context)
```

```html
<!-- articles/templates/hello.html -->

<h1>안녕하세요, {{ name }}님!</h1>
```



### Django Template Language

> django template에서 사용하는 built-in template system이며,  조건, 반복, 변수 치환, 필터 등의 기능을 제공
>
> 프로그래밍적 로직이 아니라 프레젠테이션을 표현하기 위한 것
>
> https://docs.djangoproject.com/ko/3.1/ref/templates/language/#tags
>
> https://docs.djangoproject.com/ko/3.1/ref/templates/builtins/#built-in-template-tags-and-filters

**syntax**

- variables
  - context에서 값을 출력하는데, context는 키를 값에 매핑하는 딕셔너리와 유사한 객체
- tags
- filters
  - 변수 및 태그 인수의 값을 변환
- comments



---



## HTML Form

**Form**

- 웹에서 사용자 정보를 입력하는 여러(text, button, checkbox, file, hidden, image, password, radio, reset, submit) 방식을 제공하고, **사용자로부터 할당된 데이터를 서버로 전송하는 역할**을 담당하는 HTML 태그
- form은 한 페이지에서 다른 페이지로 데이터를 전달하기 위해서 사용한다.
- 핵심 속성 2가지
  - **action** —> 입력 데이터가 **전송될 URL** 지정
  - **method** —> 입력 데이터 **전달 방식** 지정



**Input**

- form 태그 중에서 가장 중요한 태그로 **사용자로부터 데이터를 입력 받기 위해** 사용된다.
- input 태그의 속성
  - `name` 
    - 중복 가능, form을 제출했을 때 name이라는 이름에 설정된 값을 넘겨서 값을 가져올 수 있다. 
    - 주요 용도는 GET/POST 방식으로 서버에 전달하는 파라미터(name 은 key , value 는 value)로 `?key=value&key=value` 형태로 전달된다.



**HTTP method `GET`**

> https://developer.mozilla.org/ko/docs/Web/HTTP/Methods

- 서버로부터 **정보를 조회**하는 데 사용한다. 
- 데이터를 서버로 전송할 때 body가 아닌 **`쿼리 스트링`을 통해 전송**한다. (url에서 확인 가능)
- **서버의 데이터나 상태를 변경 시키지 않아야 하기 때문에 조회(html)를 할 때 사용**한다. 
- 우리는 서버에 요청을 하면 HTML 문서 파일 한 장을 받는데 이때 사용하는 요청의 방식이 GET 방식이다.



**throw & catch**

- throw

  ```python
  # first_project/urls.py
  
  path('throw/', views.throw),
  ```

  ```python
  # articles/views.py 
  
  def throw(request):
      return render(request, 'throw.html')
  ```

  ```html
  <!-- articles/templates/throw.html -->
  
  <form action="/catch/" method="GET">
    <label for="message">Throw</label>
    <input type="text" id="message" name="message">
    <input type="submit">
  </form>
  ```

- catch

  ```python
  # first_project/urls.py
  
  path('catch/', views.catch),
  ```

  ```python
  # articles/views.py
  
  def catch(request):
      message = request.GET.get('message')
      context = {
          'message': message,
      }
      return render(request, 'catch.html', context)
  ```

  ```django
  <!-- articles/templates/catch.html -->
  
  <h1>너가 던져서 내가 받은건 {{ message }}야!</h1>
  <a href="/throw/">뒤로</a>
  ```



**Request**

> https://docs.djangoproject.com/en/3.1/ref/request-response/#module-django.http

- 요청 간의 모든 정보를 담고 있는 변수
- 페이지가 요청되면 Django는 요청에 대한 메타 데이터를 포함하는 HttpRequest 객체를 만든다.
- 그런 다음 Django는 적절한 view 함수를 로드하고 HttpRequest를 뷰 함수의 첫 번째 인수로 전달합니다. 
- 그리고 각 view는 HttpResponse 개체를 반환한다.



---



## URL 분리

> 각 app 폴더에 urls.py를 각각 작성함으로써 코드 유지보수에 긍정적인 구조로 변경



**두번째 app 생성 및 등록**

```bash
$ python manage.py startapp pages
```

```python
INSTALLED_APPS = [
    'articles',
    'pages',
    ...,
]
```



**프로젝트 urls.py**

```python
# firstapp/urls.py

from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
]
```



`include()`

- 다른 URLconf(app1/urls.py)들을 참조할 수 있도록 도와준다.
- Django가 함수 `include()`를 만나게 되면, URL의 그 시점까지 일치하는 부분을 잘라내고, 남은 문자열 부분을 후속 처리를 위해 include 된 URLconf로 전달한다.



---



## URL Name

> path() 함수의 name value를 작성해 `{% url %}` template tag로 호출



**url template tag**

> https://docs.djangoproject.com/en/3.1/ref/templates/builtins/#url

- django 는 path() 함수에서 name 인수(optional) 를 정의해, `{% url %}` template tag 를 사용하여 URL 설정에 정의된 특정한 URL 경로들의 의존성을 제거할 수 있다.

  ```python
  # articles/urls.py
  
  urlpatterns = [
      path('index/', views.index, name='index'),
      path('dinner/', views.dinner, name='dinner'),
      path('hello/<str:name>/', views.hello, name='hello'),
      path('dtl-practice/', views.dtl_practice, name='dtl_practice'),
      path('throw/', views.throw, name='throw'),
      path('catch/', views.catch, name='catch'),
  ]
  ```

  ```django
  <!-- throw.html -->
  
  <body>
    <h1>Throw 페이지</h1>
    <form action="{% url 'catch' %}" method="GET">
      <label for="name">데이터 입력 : </label>
      <input type="text" id="name" name="name">
      <input type="submit">
    </form>
  </body>
  ```



---



## URL Namespace

- 예를 들어, articles app은 index 이라는 view를 가지고 있고, 동일한 프로젝트에 다른 app 에서도 index 이라는 view를 가지고 동일한 url name 을 사용할 수도 있다. 과연 Django가 `{% url 'index' %}` 처럼 사용할 때, 어떤 app 의 view 에서 URL을 생성할지 알 수 있을까?

  ```python
  # articles/urls.py
  
  app_name = 'articles'
  urlpatterns = [
      ...
  ]
  ```

  ```python
  # pages/urls.py
  
  app_name = 'pages'
  urlpatterns = [
  ]
  ```

  - urls.py 에 app_name 을 통해 app 의 이름공간을 설정한다.
  - 이제 기존 모든 url 은 다음과 같이 변경할 수 있다.

  ```django
  <!-- throw.html -->
  
  <form action="{% url 'articles:catch' %}" method="GET">
    ...
  </form>
  ```

  

---



## Django Namespace

> Namespace
>
> 이름공간 또는 네임스페이스(Namespace)는 객체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 객체만을 가리키게 된다.
>
> django에서는 서로 다른 app의 같은 이름을 가진 url name은 app_name을 설정해서 구분하고,
>
> templates, static 등 django는 정해진 경로 하나로 모아서 보기 때문에 중간에 폴더를 임의로 만들어 줌으로써 이름공간을 설정한다.



**파일트리 예시**

```
├── articles
│   ├── templates
│   │   └── articles
│   │       ├── catch.html
│   │       ├── dinner.html
│   │       ├── dtl_practice.html
│   │       ├── hello.html
│   │       ├── index.html
│   │       └── throw.html
```

```python
# articles/views.py 

return render(request, 'articles/index.html')
```



---



## Template Inheritance

> https://docs.djangoproject.com/ko/3.1/ref/templates/language/#template-inheritance



**템플릿 상속**

- 템플릿 상속을 사용하면 사이트의 모든 공통 요소를 포함하고, 하위 템플릿이 재정의(override) 할 수있는 블록(block)을 정의하는 기본 "스켈레톤" 템플릿을 만들 수 있다.

- 템플릿 상속은 기본적으로 코드의 재사용성에 초점을 맞춘다. 



**작성**

- `base.html` 파일을 `firstapp/templates/base.html` 에 생성 해보자.

- Django는 기본적으로 `app_name/templates` 를 바라보게 설정되어있다. (`APP_DIRS=True` 설정)

- 우리가 옮긴 위치는 `project폴더/templates` 이므로, Django는 현재 상태에서 해당 template 파일을 찾을 수 없다.

- 각 앱 내의 `templates` 폴더가 아닌 임의의 위치에 있는 template을 읽기 위해서는 Django에서 그 위치를 알려줘야 한다.

  ```python
  TEMPLATES = [
      {
          'BACKEND': 'django.template.backends.django.DjangoTemplates',
          'DIRS': [BASE_DIR / 'first_project' / 'templates'],
          ...,
  ]
  ```



**[참고] Path()** 

> os 마다 경로를 표기하는 `/` , `\` 로 다를 수 있음. (ex. WINDOWS) 
>
> 어떤 환경에서건 `/` 로 경로 표기(unix path)를 통일하기 위해 사용
>
> https://docs.python.org/ko/3/library/pathlib.html#module-pathlib



**템플릿 상속을 위한 기본 세팅**

- 프로젝트 폴더에서 `templates` 폴더 만든 후에 `base.html` 파일 생성

  ```django
  <!-- firstapp/templates/base.html -->
  
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="<https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css>" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
    <title>Document</title>
  </head>
  <body>
    <h1 class="text-center">Template Inheritance</h1>
    <hr>
    <div class="container">
      {% block content %}
      {% endblock %}
    </div>
    <script src="<https://code.jquery.com/jquery-3.5.1.slim.min.js>" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="<https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js>" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
    <script src="<https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js>" integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV" crossorigin="anonymous"></script>
  </body>
  </html>
  ```



**`block` tag**

- 하위 템플릿에서 재 지정(overriden)할 수있는 블록을 정의
- 하위 템플릿이 채울 수 있는 공간



**`extends` tag**

> https://docs.djangoproject.com/ko/3.1/ref/templates/builtins/#std:templatetag-extends

- 이(자식) 템플릿이 부모 템플릿을 확장한다는 것을 알림

- `{% extends '' %}` 는 반드시 문서의 최상단에 위치해야 한다.

  ```django
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>안녕하세요! 반갑습니다!!</h1>
  {% endblock %}
  ```



**django 설계 철학 (Template)**

> https://docs.djangoproject.com/ko/3.1/misc/design-philosophies/#template-system

- 표현과 로직(view)을 분리
  - 우리는 템플릿 시스템이 `표현`을 제어하는 도구이자 표현에 관련된 로직일 뿐이라고 본다. 
  - 템플릿 시스템은 이러한 기본 목표를 넘어서는 기능을 지원하지 말아야 한다,

- 중복을 배제
  - 대다수의 동적 웹사이트는 공통 헤더, 푸터, 네이게이션 바 같은 사이트 공통 디자인을 갖는다. 

    Django 템플릿 시스템은 이러한 요소를 한 곳에 저장하기 쉽게 하여 중복 코드를 없애야 한다.

  - 이것이 `템플릿 상속`의 기초가 되는 철학