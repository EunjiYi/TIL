[TOC]

# 03_django_form

> 2020.09.14

## Intro

> Form은 Django 프로젝트의 주요 유효성 검사 도구들 중 하나이며, 공격 및 우연한 데이터 손상에 대한 중요한 방어 수단이다.

- 우리는 지금까지 HTML form, input을 통해서 사용자로부터 데이터를 받았다.
- 이렇게 직접 사용자의 데이터를 받으면 입력된 데이터의 유효성을 검증하고, 필요시에 입력된 데이터를 검증 결과와 함께 다시 표시하며, 유효한 데이터에 대해 요구되는 동작을 수행하는 것을 "올바르게 하기" 위해서는 꽤 많은 노력이 필요한 작업이다.
- Django는 일부 과중한 작업과 반복 코드를 줄여줌으로써, 이 작업을 훨씬 쉽게 만든다.

<br>

## Django's role in forms

Django는 forms에 관련된 작업의 세 부분을 처리한다.

1. 렌더링을 위한 데이터 준비 및 재구성
2. 데이터에 대한 HTML forms 생성
3. 클라이언트로 부터 받은 데이터 수신 및 처리

이 모든 작업을 수동으로 수행하는 코드를 작성할 수 있지만 Django가 모든 작업을 처리 할 수 있다.

<br>

## Form Class

> https://docs.djangoproject.com/ko/3.1/topics/forms/#working-with-forms

- `Form` 클래스는 Django form 관리 시스템의 핵심이다. Form 클래스는 form내 field들, field 배치, 디스플레이 widget, label, 초기값, 유효한 값과 (유효성 체크이후에) 비유효 field에 관련된 에러메시지를 결정한다.

<br>

**들어가기 전**

- `Form` 을 선언하는 문법은 `Model` 을 선언하는 것과 비슷하고 같은 필드 타입을 사용한다. (또한, 일부 매개변수도 유사하다.)

<br>

**Form 선언**

- Form을 생성하기 위해, Form클래스에서 파생된, `forms` 라이브러리를 import 하고 폼 필드를 생성한다.

- app 폴더에 `forms.py` 파일을 작성한다.

- view까지 작성해서 템플릿에서 출력까지 확인.

  ```python
  # articles/forms.py
  
  from django import forms
  
  class ArticleForm(forms.Form):
      title = forms.CharField(max_length=10)
      content = forms.CharField(widget=forms.Textarea)
  ```

  ```python
  # articles/views.py
  
  from .forms import ArticleForm
  
  def new(request):
      form = ArticleForm()
      context = {
          'form': form,
      }
      return render(request, 'articles/new.html', context)
  ```

  ```django
  <!-- articles/new.html -->
  
  {% extends 'base.html' %}
  
  {% block content %}
    <h1 class="text-center">NEW</h1>
    <form action="{% url 'articles:create' %}" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
    <hr>
    <a href="{% url 'articles:index' %}">[back]</a>
  {% endblock %}
  ```

  - 개발자 도구로 만들어진 input 태그 확인해보자.

<br>

**Outputting forms as HTML**

> https://docs.djangoproject.com/ko/2.2/ref/forms/api/#outputting-forms-as-html

- `as_p()` : 각 필드가 단락(paragraph)으로 렌더링
- `as_ul()` : 각 필드가 목록항목(list item)으로 렌더링
- `as_table()` : 각 필드가 테이블 행으로 렌더링

<br>

**form fields**

- 입력 유효성 검사 논리를 처리하며 템플릿에서 직접 사용됨

<br>

**widget**

> Django의 HTML input 요소 표현
> https://docs.djangoproject.com/en/3.1/ref/forms/widgets/#module-django.forms.widgets

- 하지만 위젯은 반드시 form fields에 할당 됨을 주의하자

- 위젯을 form fields와 혼동해서는 안된다

<br>

### Form fields

> https://docs.djangoproject.com/en/3.1/ref/forms/fields/#module-django.forms.fields

```python
class ArticleForm(forms.Form):
    REGION_A = 'sl'
    REGION_B = 'dj'
    REGION_C = 'gj'
    REGION_D = 'gm'
    REGIONS = [
        (REGION_A, '서울'),
        (REGION_B, '대전'),
        (REGION_C, '광주'),
        (REGION_D, '구미'),
    ]
    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
    region = forms.ChoiceField(choices=REGIONS, widget=forms.RadioSelect())
```



---



## ModelForm

> https://docs.djangoproject.com/en/3.1/topics/forms/modelforms/#creating-forms-from-models

- FormClass 에서는 Model에서 이미 정의한 필드를 반복해서 정의해야 했다. 

- 하지만 Model에 이미 필드를 정의했기 때문에 다시 필드 유형을 정의하는 것은 불필요하다.

- Django에서는 model을 통해 Form Class를 만들수 있는 Helper(도우미)를 제공한다.

  - ModelForm Helper 클래스를 사용하여 모델에서 form을 작성
  - ModelForm은 일반 Form과 완전히 같은 방식(객체생성)으로 view에서 사용할 수 있다.

  ```python
  # articles/forms.py
  
  from django import forms
  from .models import Article
  
  # class ArticleForm(forms.Form):
  #     title = forms.CharField(max_length=10)
  #     content = forms.CharField(widget=forms.Textarea)
  
  class ArticleForm(forms.ModelForm):
      
      class Meta:
          model = Article
          fields = '__all__'
          # exclude = ('title',)
  ```

  - 기존 ArticleForm 클래스를 주석 처리하고, 같은 이름의 새로운 클래스를 정의한다.
  - `new.html` 변화 확인

<br>

### CREATE

```python
# articles/views.py

def create(request):
    form = ArticleForm(request.POST) 
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)
    return redirect('articles:new')
```

<br>

**The `save()` method**

> https://docs.djangoproject.com/ko/3.1/topics/forms/modelforms/#the-save-method

<br>

### view 합치기

> https://docs.djangoproject.com/en/3.1/topics/forms/#the-view

```python
# articles/forms.py

def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST) 
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)
```

>  new view 함수, url 삭제
>
> new.html → `create.html` 이름변경

```django
<!-- articles/create.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">CREATE</h1>
  <form action="" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```

- input 태그에 데이터를 공백으로 넣어보고 글 작성 후 에러 메세지 출력 확인

<br>

### Widgets

> https://docs.djangoproject.com/en/3.1/ref/forms/widgets/#module-django.forms.widgets

1. 첫번째 방식

   ```python
   class ArticleForm(forms.ModelForm):
   
       class Meta:
           model = Article
           fields = '__all__'
           widgets = {
               'title': forms.TextInput(attrs={
                   'class': 'title',
                   'placeholder': 'Enter the title',
                   'maxlength': 10,
                   }
               )
           }
   ```

2. 두번째 방식 **(권장)**

   ```python
   class ArticleForm(forms.ModelForm):
       title = forms.CharField(
           label='제목',
           widget=forms.TextInput(
               attrs={
                   'class': 'my-title', 
                   'placeholder': 'Enter the title',
               }
           ),
       )
   
       class Meta:
           model = Article
           fields = '__all__'
   ```

   ```python
   class ArticleForm(forms.ModelForm):
       title = forms.CharField(
           label='제목',
           widget=forms.TextInput(
               attrs={
                   'class': 'my-title', 
                   'placeholder': 'Enter the title',
                   'maxlength': 10,
               }
           ),
       )
       content = forms.CharField(
           label='내용',
           widget=forms.Textarea(
               attrs={
                   'class': 'my-content',
                   'placeholder': 'Enter the content',
                   'rows': 5,
                   'cols': 50,
               }
           ),
           error_messages={
               'required': 'Please enter your content'
           }
       )
       
       class Meta:
           model = Article
           fields = '__all__'
   ```

<br>

**Form과 ModelForm의 핵심 차이점**

- Form
  - 어떤 모델에 저장해야 하는지 알 수 없기 때문에 유효성 검사를 하고 실제로 DB에 저장할 때는  `cleaned_data` 와 `article = Article(title=title, content=content)` 를 사용해서 따로 `.save()` 를 해야 한다.
  - Form Class가 `cleaned_data` 로 딕셔너리로 만들어서 제공해 주고, 우리는 `.get` 으로 가져와서 Article 을 만드는데 사용한다.
- ModelForm
  - django 가 해당 모델에서 양식에 필요한 대부분의 정보를 이미 정의한다.
  - `forms.py` 에 Meta 정보로 `models.py` 에 이미 정의한  Article 을 넘겼기 때문에 어떤 모델에 레코드를 만들어야 할지 알고 있어서 바로 `.save()` 가 가능하다.

<br>

### Update

> https://docs.djangoproject.com/ko/3.1/topics/forms/modelforms/#the-save-method

- 인자 `instance`는 **수정 대상이 되는 객체를 지정**한다.

- create 로직과 다른 점은 기존의 데이터를 가져와 수정을 한다는 점이다. 

  `article` 인스턴스를 DB에서 가져와, ArticleForm에  `instance` 의 인자로 넣는다.

  - `request.POST` : 사용자가 form을 통해 전송한 데이터
  - `instance` : 수정이 되는 대상

```python
# articles/urls.py

path('<int:pk>/update/', views.update, name='update'),
```

```python
# articles/views.py

def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        # Create a form to edit an existing Article,
        # but use POST data to populate the form.
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
    else:
        # Creating a form to change an existing article.
        form = ArticleForm(instance=article)
    context = {
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```

```django
<!-- articles/update.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">UPDATE</h1>
  <form action="" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:detail' aritlce.pk %}">[back]</a>
{% endblock %}
```

```django
<!-- articles/detail.html -->

<a href="{% url 'articles:update' article.pk %}">UPDATE</a><br>
```

```python
# articles/views.py

def update(request, pk):
    ...
    context = {
        'form': form,
        'article': article,
    }
    return render(request, 'articles/update.html', context)
```

<br>

### Delete

```python
# articles/urls.py

path('<int:pk>/delete/', views.delete, name='delete'),
```

```python
# articles/views.py

def delete(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    return redirect('articles:detail', article.pk)
```

```django
<!-- articles/detail.html -->

<form action="{% url 'articles:delete' article.pk %}" method="POST">
  {% csrf_token %}
  <input type="submit" value="DELETE">
</form>
```



**`forms.py`  위치**

- Form class는 `forms.py` 뿐만 아니라 다른 위치 어느 곳에 두어도 상관없다.
- 하지만 되도록 app 폴더에 두며, Form class는 `forms.py` 에 작성하는 것이 일반적인 컨벤션이다.



------



## Rendering fields manually

> https://docs.djangoproject.com/ko/3.1/topics/forms/#rendering-fields-manually

<br>

### Form 분리

- template에서 `{{ form }}` 으로 한번에 사용해서 커스터마이징이 힘들었는데, 다양한 방법으로 사용도 가능하다.

1. Rendering fields manually

   ```django
   <!-- articles/create.html --> 
   
   <h1>CREATE</h1>
   ...
   <hr>
   
   <form action="" method="POST">
     {% csrf_token %}
     <div>
       {{ form.title.errors }}
       {{ form.title.label_tag }}
       {{ form.title }}
     </div>
     <div>
       {{ form.content.errors }}
       {{ form.content.label_tag }}
       {{ form.content }}
     </div>
     <button class="btn btn-primary">작성</button>
   </form>
   ```

2. Looping over the form’s fields (`{% for %}`)

   ```django
   <!-- articles/create.html --> 
   
   ...
   
   <hr>
   
   <form action="" method="POST">
     {% csrf_token %}
     {% for field in form %}
       {{ field.errors }}
       {{ field.label_tag }}
       {{ field }}
     {% endfor %}
     <button class="btn btn-primary">작성</button>
   </form>
   ```

<br>

### Bootstrap Form

> https://getbootstrap.com/docs/4.5/components/forms/

- `form-group`, `form-control` 두 class name이 핵심이다.

  ```django
  <!-- articles/create.html -->
  
  ...
  
  <hr>
  
  <h2>Bootstrap Form</h2>
  <form action="" method="POST">
    {% csrf_token %}
    {% for field in form %}
      <div class="form-group">
        {{ field.errors }}
        {{ field.label_tag }} 
        {{ field }}
      </div>
    {% endfor %}
    <button class="btn btn-primary">작성</button>
  </form>
  ```

  ```python
  # articles/forms.py
  
  class ArticleForm(forms.ModelForm):
      title = forms.CharField(
          label='제목',
          widget=forms.TextInput(attrs={
              'class': 'my-title form-control',
              'placeholder': 'Enter the title',
              'maxlength': 10,
          })
      )
      content = forms.CharField(
          label='내용',
          widget=forms.Textarea(attrs={
              'class': 'my-content form-control',
              'rows': 5,
              'cols': 50,
          }),
          error_messages={
              'required': '내용 넣어라...'
          }
      )
  ```

<br>

### Error message with Bootstrap

```django
<form action="" method="POST">
  {% csrf_token %}
  {% for field in form %}
    {% if field.errors %}
      {% for error in field.errors %}
        <div class="alert alert-warning" role="alert">{{ error|escape }}</div>
      {% endfor %}
    {% endif %}
    <div class="form-group">
      {{ field.label_tag }} 
      {{ field }}
    </div>
  {% endfor %}
  <button class="btn btn-primary">작성</button>
</form>
```



---



## Django Bootstrap Library

<br>

### django-bootstrap4

> https://django-bootstrap4.readthedocs.io/en/latest/installation.html
>
> https://pypi.org/project/django-bootstrap4/

- 공식 문서를 같이 따라가며 이것저것 사용 해보자.

  ```bash
  $ pip install django-bootstrap4
  ```

  ```python
  # settings.py
  
  INSTALLED_APPS = [
    ...
    'bootstrap4',
  	...
  ]
  ```

  ```bash
  $ pip freeze > requirements.txt
  ```

  ```django
  <!-- articles/base.html -->
  
  {% load bootstrap4 %}
  
  <!DOCTYPE html>
  <html lang="ko">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    {% bootstrap_css %}
    <title>Document</title>
  </head>
  <body>
    <div class="container">
      {% block content %}
      {% endblock %}
    </div>
    {% bootstrap_javascript jquery='full' %}
  </body>
  </html>
  ```

  ```django
  <!-- articles/update.html -->
  
  {% extends 'base.html' %}
  {% load bootstrap4 %}
  
  {% block content %}
    ...
    <form action="" method="POST">
      {% csrf_token %}
      {% bootstrap_form form layout='horizontal' %}
      {% buttons submit="Submit" reset="Cancel" %}{% endbuttons %}
    </form>
    ...
    {% endif %}
  {% endblock %}
  ```



---



## View decorators

> https://docs.djangoproject.com/en/3.1/topics/http/decorators/#module-django.views.decorators.http

<br>

**데코레이터(decorator)**

- 어떤 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능을 `연장`하게 해주는 `함수`

- Django는 다양한 HTTP 기능을 지원하기 위해 뷰에 적용 할 수있는 여러 데코레이터를 제공

<br>

### Allowed HTTP methods

> 일치하지 않는 메서드 요청이라면 `405 Method Not Allowed` 에러를 발생

<br>

**`require_http_methods()`**

- view가 특정한 요청 method만 허용하도록하는 데코레이터

  ```python
  from django.views.decorators.http import require_http_methods, require_POST
  
  
  @require_http_methods(['GET', 'POST'])
  def create(request):
      pass
  
    
  @require_http_methods(['GET', 'POST'])
  def update(request, pk):
      pass
  ```

**`require_POST()`**

- view가 POST 메서드만 요청만 승인하도록 하는 데코레이터

  ```python
  from django.views.decorators.http import require_http_methods, require_POST
  
  
  @require_POST
  def delete(request, pk):
      article = Article.objects.get(pk=pk)
      article.delete()
      return redirect('articles:index')
  ```

  - url 로 delete 시도 후 405 에러페이지 & terminal 로그 확인하기



------