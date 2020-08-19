aclass@M805 MINGW64 ~/Desktop/0818/0814_ws_solution/intro_pjt
$ python manage.py shell





```
주피터노트북 설치하면 
ipython이 같이 설치되는데 그거 보면 이렇게 나옴.
```

```
aclass@M805 MINGW64 ~/Desktop/0818/0814_ws_solution/intro_pjt
$ python manage.py shell
Python 3.7.7 (tags/v3.7.7:d7c567b08f, Mar 10 2020, 10:41:24) [MSC v.1900 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.16.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: 

```



```
In [1]: from pathlib import Path

In [2]: p = Path()

In [4]: p
Out[4]: WindowsPath('.')

In [5]: p.resolve()
Out[5]: WindowsPath('C:/Users/aclass/Desktop/0818/0814_ws_solution/intro_pjt') #현재 경로에 대한 절대경로 정보

In [6]: p.resolve().parent
Out[6]: WindowsPath('C:/Users/aclass/Desktop/0818/0814_ws_solution') #부모경로 출력
```



#### 템플릿 확장하기

1. base.html 생성하기

2. base.html을 settings.py에 등록하기

3. 상속하려는 템플릿에서 첫번째 줄에 `{% extends 'base.html' %}` 선언하기

4. {% block 블럭명 %} {% endblock %} 사이에 코드 작성하기.



#### 장고개발을 위한 준비

1. 프로젝트 생성

2. app 생성

3. url 분리

4. base.html 준비

   -> 이 1~4단계가 기본준비.

5. url 수정 -> views.py수정 -> template 준비하고 이거 반복







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