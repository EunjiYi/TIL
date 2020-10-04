[TOC]

# 02_django_crud

> 2020.08.21

## 사전 준비

**템플릿 폴더 구조 및 url 분리**

- `articles/urls.py` 파일 생성

  ```python
  # articles/urls.py
  
  from django.urls import path
  
  app_name = 'articles'
  urlpatterns = [
    
  ]
  ```

- 프로젝트 폴더 url 설정

  ```python
  # crud/urls.py
  
  from django.contrib import admin
  from django.urls import path, include
  
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('articles/', include('articles.urls')),
  ]
  ```



**`base.html` 설정**

```html
<!-- crud/templates/base.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="<https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css>"
    integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
</head>
<body>
  <div class="container">
    {% block content %}
    {% endblock %}
  </div>
  <script src="<https://code.jquery.com/jquery-3.5.1.slim.min.js>"
    integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous">
  </script>
  <script src="<https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js>"
    integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous">
  </script>
  <script src="<https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/js/bootstrap.min.js>"
    integrity="sha384-OgVRvuATP1z7JjHLkuOU7Xw704+h835Lr+6QL9UvYjZE3Ipu6Tp75j7Bh/kR0JKI" crossorigin="anonymous">
  </script>
</body>
</html>
```

```python
# crud/settings.py

'DIRS': [BASE_DIR / 'crud' / 'templates'],
```



**기본 페이지 설정**

```python
# articles/urls.py

from django.urls import path
from . import views

app_name = 'articles'
urlpatterns = [
    path('', views.index, name='index'),
]



# articles/views.py

def index(request):
    return render(request, 'articles/index.html')
```

```django
<!-- templates/articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">Articles</h1>
{% endblock %}
```



---



## READ

**`index.html` 수정**

```python
# articles/views.py

from .models import Article

def index(request):
    articles = Article.objects.all()
    context = {
        'articles': articles,
    }
    return render(request, 'articles/index.html', context)
```

```django
<!--templates/articles/index.html-->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">Articles</h1>
  <hr>
  {% for article in articles %}
    <p>글 번호: {{ article.pk }}</p>
    <p>글 제목: {{ article.title }}</p>
    <p>글 내용: {{ article.content }}</p>
    <hr>
  {% endfor %}
{% endblock %}
```



---



## CREATE

### New

```python
# articles/urls.py

path('new/', views.new, name='new'),
```

```python
# articles/views.py

def new(request):
    return render(request, 'articles/new.html')
```

```django
<!-- templates/articles/new.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">NEW</h1>
  <form action="#" method="GET">
    <label for="title">Title: </label>
    <input type="text" name="title"><br>
    <label for="content">Content: </label>
    <textarea name="content" cols="30" rows="5"></textarea><br>
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```

```django
<!-- templates/articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">Articles</h1>
  <a href="{% url 'articles:new' %}">[new]</a>
  <hr>
{% endblock  %}
```



### Create

```python
# article/urls.py

path('create/', views.create, name='create'),
```

```python
def create(request):
    title = request.GET.get('title') 
    content = request.GET.get('content')
    
		# 1.
		# article = Article()
		# article.title = title
		# article.content = content
		# article.save()
		
		# 2. 
    # Article.objects.create(title=title, content=content)
		
    article = Article(title=title, content=content)
    article.save()
    return render(request, 'articles/create.html')
```

```django
<!-- templates/articles/create.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class='text-center'>성공적으로 글이 작성되었습니다.</h1>
{% endblock %}
```

```django
<!-- templates/articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">NEW</h1>
  <form action="{% url 'articles:create' %}" method="GET">
    <label for="title">Title: </label>
    <input type="text" name="title"><br>
    <label for="content">Content: </label>
    <textarea name="content" cols="30" rows="5"></textarea><br>
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```





**게시글 순서 변경**

- `#1`의 경우 기존에 DB에 있는 순서를 파이썬이 변경한다.

- `#2` 의 경우 DB가 변경한다. (2번으로 진행)

- 참고로, `order_by` 는 단일 쿼리에서는 사용할 수 없고 QuerySet 에서만 사용 가능하다.

  ```python
  # articles/views.py
  
  def index(request):
      # 1. articles = Article.objects.all()[::-1]	 # 파이썬이 변경
      # 2. articles = Artile.objects.order_by('-pk')  # DB에서 변경
  ```

- 이제 글이 작성되면 create.html 이 아닌 index.html 로 돌아가게 해보자

  ```python
  # articles/views.py
  
  def create(request):
      ...
      
      return render(request, 'articles/index.html')
  ```



---



### Http Method POST

> https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/POST

- **3가지 이유에서 우리는 글을 작성할 때 GET 요청이 아닌 POST 요청을 해야 한다.**

  1. 사용자는 Django에게 '**HTML 파일 줘(GET)**' 가 아니라 '**~한 레코드(글)을 생성해(POST)**' 이기 때문에 GET보다는 POST 요청이 맞다.

  2. 데이터는 URL에 직접 노출되면 안된다. (우리가 주소창으로 접근하는 방식은 모두 GET 요청) query의 형태를 통해 DB schema를 유추할 수 있다.

  3. 모델(DB)을 건드리는 친구는 GET이 아닌 POST 요청! 왜? 중요하니까 **최소한의 신원 확인**이 필요하다!

     

- POST 요청은 리소스를 생성/변경하기 위해 데이터를 HTTP body에 담아 전송한다.
  - GET → CRUD에서 R에 해당
  - POST → CRUD에서 C/U/D에 해당
- 서버의 데이터나 상태를 변경 시키기 때문에 동일한 요청을 여러 번 전송해도 응답의 결과는 다를 수 있다.
  - 데이터가 생성/수정/삭제 되기 때문에 매번 다른 응답이 온다.
- 원칙적으로 POST 요청을 html 파일로 응답하면 안된다.
  - GET 요청만 html 파일로 응답하고 POST 요청은 GET 요청을 받는 페이지로 **redirect** 해야 한다.



**DB 조작(GET/POST)**

- GET 요청은 DB에서 데이터를 꺼내서 가져온다. 즉, DB에 변화를 주는 게 아니다.
  - 즉, **GET**은 누가 요청해도 어차피 정보를 조회(HTML 파일을 얻는 것)하기 때문에 문제가 되지 않음.
- POST 요청은 DB에 조작(생성/수정/삭제)를 하는 것(디비에 변화를 준다)
  - **POST**는 DB에 조작이 가해지기 때문에 요청자에 대한 최소한의 검증을 하지 않으면 아무나 DB에 접근해서 데이터에 조작을 가할 수 있다.
  - `csrf_token`을 통해서 요청자의 최소한의 신원확인을 한다.



**`new.html` 수정**

```django
<!-- templates/articles/new.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">NEW</h1>
  <form action="{% url 'articles:create' %}" **method="POST"**>
    {% csrf_token %}
    <label for="title">Title: </label>
    <input type="text" name="title"><br>
    <label for="content">Content: </label>
    <textarea name="content" cols="30" rows="5"></textarea><br>
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```

```python
# articles/views.py

def create(request):
    title = request.POST.get('title') 
    content = request.POST.get('content') 

    article = Article(title=title, content=content)
    article.save()
    return render(request, 'articles/index.html')
```





**CSRF Token**

> [https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0](https://ko.wikipedia.org/wiki/사이트_간_요청_위조)

- 사이트 간 요청 위조(Cross-Site-Request-Fogery)

  - 웹 애플리케이션 취약점 중 하나로 **사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법**을 의미한다.
  - `{% csrf_token %}` 을 설정하면 input type hidden 으로 특정한 hash 값이 들어있다.

- `{% csrf_token %}` 이 없다면?

  - `403 forbidden` 에러: 서버에 요청은 도달했으나 서버가 접근을 거부할 때 반환하는 HTTP 응답 코드 / 오류 코드. 서버 자체 또는 서버에 있는 파일에 접근할 권한이 없을 경우에 발생
  - 이러한 접근을 할 수 있도록 하는 것이 `{% csrf_token %}` → 사내 인트라넷 서버를 사내가 아닌 밖에서 접속하려고 할 때도 해당 HTTP 응답 코드가 뜬다.

- 해당 csrf attack 보안과 관련된 설정은 `settings.py`에서 `MIDDLEWARE` 에 되어있음

  - 실제로 요청 과정에서 `urls.py` 이전에 Middleware의 설정 사항들을 순차적으로 거친다. 응답은 아래에서 위로부터 미들웨어를 적용시킨다.

  ```python
  # settings.py
  
  MIDDLEWARE = [
  	'django.middleware.csrf.CsrfViewMiddleware',
  ]
  ```

  

### Redirect

- POST 요청은 HTML 문서를 렌더링 하는 것이 아니라 **'~~ 좀 처리해줘(요청)'의 의미이기 때문에 요청을 처리하고 나서의 요청의 결과를 보기 위한 페이지로 바로 넘겨주는 것이 일반적**이다.

  ```python
  # articles/views.py
  
  from django.shortcuts import render, redirect
  
  
  def create(request):
      title = request.POST.get('title') 
      content = request.POST.get('content')
      
      article = Article(title=title, content=content)
      article.save()
      return redirect('articles:index')
  ```



**POST 요청으로 변경 후 변화하는 것**

- POST 요청을 하게 되면 form을 통해 전송한 데이터를 받을 때도 `request.POST.get()` 로 받아야 함
- 글이 작성되면 실제로 주소 창에 내가 넘긴 데이터가 나타나지 않는다.(POST 요청은 HTTP body에 데이터를 전송함)
- POST는 html을 요청하는 것이 아니기 때문에 html 파일을 받아볼 수 있는 곳으로 다시 redirect 한다.



---



## DETAIL

**urls 설정**

- variable routing → 주소를 통해 요청이 들어올 때 특정 값을 변수화 시킬 수 있다.
- 우리는 pk 값을 변수화 시켜 사용할 것 pk는 detail 함수의 pk라는 이름의 인자로 넘어가게 된다!
- `index.html` 의 `<a href="/articles/{{ article.pk }}/">[글 보러가기]</a>`  부분에서 `{{ article.pk }}` 가 실제 특정 숫자(pk값)일 것이고 요청이 보내질 때 숫자로 넘어 간다.
- 그럼 path에서 해당하는 숫자는 pk라는 변수에 저장될 것이다.

```python
# articles/urls.py

path('<int:pk>/', views.detail, name='detail'),
```



**views 설정**

```python
# articles/views.py

def detail(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'articles/detail.html', context)
```



**templates 설정**

> https://docs.djangoproject.com/en/3.1/ref/templates/builtins/#date

```django
<!-- templates/articles/detail.html -->

{% extends 'base.html' %}

{% block content %}
  <h2 class='text-center'>DETAIL</h2>
  <h3>{{ article.pk }} 번째 글</h3>
  <hr>
  <p>제목: {{ article.title }}</p>
  <p>내용: {{ article.content }}</p>
  <p>작성 시각: {{ article.created_at|date:"SHORT_DATE_FORMAT" }}</p>
  <p>수정 시각: {{ article.updated_at|date:"M j, Y" }}</p>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock  %}
```

- 하지만 지금은 1번 글, 2번 글 등을 보기 위해 주소창에 `articles/1` `articles/2` 이런식으로 요청을 보내야 한다. 이를 해결 하기 위해 index 페이지에 링크를 달아보자.

  ```django
  <!-- templates/articles/index.html -->
  
  {% extends 'base.html' %}
  
  {% block content %}
  <h1 class="text-center">Articles</h1>
  <a href="{% url 'articles:new' %}">[new]</a>
  <hr>
  {% for article in articles %}
    <p>글 번호: {{ article.pk }}</p>
    <p>글 제목: {{ article.title }}</p>
    <p>글 내용: {{ article.content }}</p>
    **<a href="{% url 'articles:detail' article.pk %}">[detail]</a>**
    <hr>
  {% endfor %}
  {% endblock  %}
  ```



**`humanize` (참고)**

> https://docs.djangoproject.com/en/3.1/ref/contrib/humanize/#module-django.contrib.humanize

```django
{% extends 'base.html' %}
{% load humanize %}

{% block content %}
  <h2 class='text-center'>DETAIL</h2>
  <h3>{{ article.pk }} 번째 글</h3>
  <hr>
  <p>제목: {{ article.title }}</p>
  <p>내용: {{ article.content }}</p>
  <p>작성 시각: {{ article.created_at|naturalday }}</p>
  <p>수정 시각: {{ article.updated_at|naturaltime }}</p>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock  %}
```



**create 로직 변경**

```python
# articles/views.py

def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')

    article = Article(title=title, content=content)
    article.save()
    return redirect('articles:detail', article.pk)
```



------



## DELETE

**urls 설정**

```python
# articles/urls.py

path('<int:pk>/delete/', views.delete, name='delete'),
```



**views 설정**

```python
# articles/views.py

def delete(request, pk):
    article = Article.objects.get(pk=pk)
    article.delete()
    return redirect('articles:index')
```



**templates 설정**

```django
{% extends 'base.html' %}

{% block content %}
  <h2 class='text-center'>DETAIL</h2>
  <h3>{{ article.pk }} 번째 글</h3>
  <hr>
  <p>제목: {{ article.title }}</p>
  <p>내용: {{ article.content }}</p>
  <p>작성 시각: {{ article.created_at|date:"SHORT_DATE_FORMAT" }}</p>
  <p>수정 시각: {{ article.updated_at|date:"M, j, Y" }}</p>
  <hr>
  <a href="{% url 'articles:delete' article.pk %}">[DELETE]</a><br>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock  %}
```



**delete → POST**

```django
<!-- articles/detail.html -->

{% extends 'base.html' %}

{% block content %}
  ...
  <form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <button class="btn btn-danger">DELETE</button>
  </form><br>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```

- 그래서 POST 로 요청을 받기 위해 다음과 같이 조건을 만든다.

  ```python
  # articles/views.py
  
  def delete(request, pk):
      article = Article.objects.get(pk=pk)
      if request.method == 'POST':
          article.delete()
          return redirect('articles:index')
      else:
          return redirect('articles:detail', article.pk)
  ```



---



## UPDATE

### Edit

**urls 설정**

```python
# articles/urls.py

path('<int:pk>/edit/', views.delete, name='edit'),
```



**views 설정**

```python
# articles/views.py

def edit(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'articles/edit.html', context)
```



**templates 설정**

```django
<!-- articles/edit.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">EDIT</h1>
  <form action="#" method="POST">
    {% csrf_token %}
    <label for="title">Title: </label>
    <input type="text" name="title" value="{{ article.title }}"><br>
    <label for="content">Content: </label>
    <textarea name="content" cols="30" rows="5">{{ article.content }}</textarea><br>
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```

- `detail.html` 에 edit 으로 가는 링크 작성

  ```django
  <!-- articles/detail.html -->
  
  {% extends 'base.html' %}
  
  {% block content %}
    <h2 class='text-center'>DETAIL</h2>
    <h3>{{ article.pk }} 번째 글</h3>
    <hr>
    <p>제목: {{ article.title }}</p>
    <p>내용: {{ article.content }}</p>
    <p>작성 시각: {{ article.created_at|date:"SHORT_DATE_FORMAT" }}</p>
    <p>수정 시각: {{ article.updated_at|date:"M j, Y" }}</p>
    <hr>
    <a href="{% url 'articles:edit' article.pk %}" class="btn btn-primary">EDIT</a><br>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
      {% csrf_token %}
      <button class="btn btn-danger">DELETE</button>
    </form><br>
    <a href="{% url 'articles:detail' article.pk %}">[back]</a>
  {% endblock  %}
  ```



### Update

**urls 설정**

```python
# articles/urls.py

path('<int:pk>/update/', views.update, name='update'),
```



**views 설정**

```python
# articles/views.py

def update(request, pk):
    article = Article.objects.get(pk=pk)
    article.title = request.POST.get('title')
    article.content = request.POST.get('content')
    article.save()
    return redirect('articles:detail', article.pk)
```

```django
{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">EDIT</h1>
  <form **action="{% url 'articles:update' article.pk %}"** method="POST">
    {% csrf_token %}
    ...
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```

------