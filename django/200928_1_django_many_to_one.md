[TOC]

# 07_django_many_to_one

> 2020.09.23

<br>

## User - Article

### User model 대체

> https://docs.djangoproject.com/en/3.1/topics/auth/customizing/#auth-custom-user

- 일부 프로젝트에서는 Django의 내장 유저 모델이 제공하는 인증 요구사항이 적절하지 않을 수 있다.
- 사용자 지정 모델을 참조하는 AUTH_USER_MODEL 설정 값을 변경해 기본 유저 모델을 재정의(override) 할 수 있다.
- Django는 새 프로젝트를 시작하는 경우 기본 사용자 모델이 충분하더라도 커스텀 유저 모델을 설정하는 것을 강력하게 권장한다.
- 커스텀 유저 모델은 기본 사용자 모델과 동일하게 작동하지만 필요한 경우 나중에 맞춤 설정할 수 있기 때문이다.
- **단, 프로젝트의 첫 migrate를 실행하기 전에 완료해야 한다.**

<br>

**AUTH_USER_MODEL**

> https://docs.djangoproject.com/en/3.1/ref/settings/#auth-user-model

- User를 나타내는데 사용하는 모델
- 기본 값은 ‘auth.User’
- 주의 사항
  - 프로젝트를 진행하는 동안 (즉, 프로젝트에 의존하는 모델을 만들고 마이그레이션 한 후) 설정은 변경할 수 없다. (변경하려면 큰 노력이 필요)
  - 프로젝트 시작 시 설정하기 위한 것이고 커스텀 User로 대체하기를 참고해서 설정한다.

<br>

**`AbstractUser` vs `AbstractBaseUser`**

> [https://github.com/django/django/blob/415e899dc46c2f8d667ff11d3e54eff759eaded4/django/contrib/auth/base_user.py#L47](https://github.com/django/django/blob/415e899dc46c2f8d667ff11d3e54eff759eaded4/django/contrib/auth/base_user.py)
>  [https://github.com/django/django/blob/415e899dc46c2f8d667ff11d3e54eff759eaded4/django/contrib/auth/models.py#L290](https://github.com/django/django/blob/415e899dc46c2f8d667ff11d3e54eff759eaded4/django/contrib/auth/models.py)

<img width="250" alt="Picture1" src="https://user-images.githubusercontent.com/18046097/93996761-98afc300-fdcd-11ea-9ae7-3d8f90f102a8.png">

- `AbstractBaseUser`
  - password 와 last_login 만 기본적으로 제공
  - 자유도가 높지만 다르 필요한 필드는 모두 작성해야 함
- `AbstractUser`
  - 관리자 권한과 함께 완전한 기능을 갖춘 사용자 모델을 구현하는 기본 클래스

```python
# accounts/models.py

from django.conf import settings
from django.contrib.auth.models import AbstractUser


class User(AbstractUser):
    pass
```

```python
# settings.py

AUTH_USER_MODEL = 'accounts.User'
```

- Django는 맞춤 모델을 참조하는 `AUTH_USER_MODEL` 설정 값을 제공함으로써 기본 사용자 모델을 오버라이드하도록 할 수 있다.

```python
# accounts/admin.py

from django.contrib.auth.admin import UserAdmin
from .models import User

admin.site.register(User, UserAdmin)
```

<br>

**데이터베이스 초기화 후 마이그레이션**

1. migrations 파일 삭제
2. db.sqlite3 삭제
3. migrations

<br>

### Custom user and built-in auth forms

> https://docs.djangoproject.com/ko/3.1/topics/auth/customizing/#custom-users-and-the-built-in-auth-forms

- 유저모델 대체 후 회원가입 시 에러 발생
- AbstractBaseUser의 모든 subclass와 호환되는 forms
  - AuthenticationForm, SetPasswordForm, PasswordChangeForm, AdminPasswordChangeForm
- User와 연결되어 있어서 커스텀 유저 모델을 사용하려면 다시 작성하거나 확장해야 하는 forms (ModelForm)
  - UserCreationForm, UserChangeForm

- `UserCreateForm()` 을 재정의 해보자.

  ```python
  # accounts/forms.py
  
  from django.contrib.auth.forms import UserChangeForm, UserCreationForm
  
  
  class CustomUserCreationForm(UserCreationForm):
  
      class Meta(UserCreationForm.Meta):
          model = get_user_model()
          fields = UserCreationForm.Meta.fields + ('email',)
  ```

  ```python
  # accounts/views.py
  
  from .forms import CustomUserChangeForm, CustomUserCreationForm
  
  
  def signup(request):
      if request.user.is_authenticated:
          return redirect('articles:index')
  
      if request.method == 'POST':
          form = CustomUserCreationForm(request.POST)
          if form.is_valid():
              user = form.save()
              auth_login(request, user)
              return redirect('articles:index')
      else:
          form = CustomUserCreationForm()
      context = {
          'form': form,
      }
      return render(request, 'accounts/signup.html', context)
  ```

<br>

### User model 참조

`get_user_model()`

- User를 직접 참조하는 대신 django.contrib.auth.get_user_model()을 사용하여 사용자 모델을 참조해야 함
- 이 메서드는 현재 활성 사용자 모델 (지정된 경우 사용자 지정 사용자 모델, 그렇지 않은 경우 User)을 반환합니다.

<br>

`settings.AUTH_USER_MODEL`

- 유저 모델에 대한 외래 키 또는 다 대다 관계를 정의 할 때 사용

- 즉, models.py 에서 유저 모델을 참조할 때 사용

- Article 모델에 외래키 설정 후 마이그레이션 작업 진행

  ```python
  # articles/models.py
  
  from django.conf import settings
  
  class Article(models.Model):
  	  ...
      user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
  ```

  ```bash
  $ python manage.py makemigrations
  
  # 첫번째 상황(null 값이 허용되지 않는 user_id 가 아무 값도 없이 article 에 추가되려 하기 때문)
  $ python manage.py makemigrations
  You are trying to add a non-nullable field 'user' to article without a default; we can't do that (the database needs something to populate existing rows).
  Please select a fix:
   1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
   2) Quit, and let me add a default in models.py
  Select an option: # 1 입력하고 enter
  
  # 두번째 상황(그럼 기존 article 의 user_id 로 어떤 데이터를 넣을건지 물어봄, 현재 admin 의 pk 값인 1을 넣자)
  Please enter the default value now, as valid Python
  The datetime and django.utils.timezone modules are available, so you can do e.g. timezone.now
  Type 'exit' to exit this prompt
  >>> # 1 입력하고 enter (그럼 현재 작성된 모든 글은 admin 이 작성한 것으로 됨)
  ```

  ```bash
  $ python manage.py migrate
  ```

<br>

- 게시글을 작성하려 하면 user 를 선택 해야하는 불필요한 field 가 노출된다. 제목과 내용만 입력하도록 필드를 설정해야한다.

  ```python
  # articles/forms.py
  
  class ArticleForm(forms.ModelForm):
      ...
      class Meta:
          model = Article
          fields = ['title', 'content',]
  ```

  - 글을 작성해보면 create 시에 유저 정보가 저장되지 않기 때문에 다음과 같은 에러가 발생한다.
    - `NOT NULL constraint failed: articles_article.user_id`

<br>

### CREATE 로직 수정

- 그리고 `request.user` 라는 현재 요청의 유저 객체를 `article.user` 에 할당한다.

  ```python
  # articles/views.py
  
  @login_required
  @require_http_methods(['GET', 'POST'])
  def create(request):
      if request.method == 'POST':
          form = ArticleForm(request.POST)
          if form.is_valid():
              article = form.save(commit=False)
              article.user = request.user
              article.save()
              return redirect('articles:detail', article.pk)
  ```

- 게시글을 작성한 user가 누구인지 보기 위해 `index.html` 수정

  ```django
  <!-- articles/index.html -->
  
  {% extends 'base.html' %}
  
  {% block content %}
    ...
    {% for article in articles %}
      <p><b>작성자 : {{ article.user }}</b></p>
      <p>글 번호 : {{ article.pk }}</p>
      <p>글 제목 : {{ article.title }}</p>
      <p>글 내용 : {{ article.content }}</p>
      <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
      <hr>
    {% endfor %}
  {% endblock %}
  ```

<br>

### UPDATE, DELETE 로직 수정

- 사용자가 자신의 글만 **수정/삭제** 할 수 있도록 내부(update/delete) 로직 수정

  ```python
  # articles/views.py
  
  from django.contrib.auth.decorators import login_required
  
  @login_required
  @require_http_methods(['GET', 'POST'])
  def update(request, pk):
      article = get_object_or_404(Article, pk=pk)
      if request.user == article.user:
          if request.method == 'POST':
              form = ArticleForm(request.POST, instance=article)
              if form.is_valid():
                  form.save()
                  return redirect('articles:detail', article.pk)
          else:
              form = ArticleForm(instance=article)
      else:
          return redirect('articles:index')
      context = {
          'form': form,
      }
      return render(request, 'articles/update.html', context)
  ```

  ```python
  # articles/views.py
  
  @require_POST
  def delete(request, pk):
      if request.user.is_authenticated:
          article = get_object_or_404(Article, pk=pk)
          if request.user == article.user:
              article.delete()
              return redirect('articles:index')
      return redirect('articles:detail', article.pk)
  ```

- 해당 게시글의 게시자가 아니라면, 삭제/수정 버튼을 보이지 않게하자.

  ```django
  <!-- articles/detail.html -->
  
  {% extends 'articles/base.html' %}
  {% block content %}
    ...
    {% if request.user == article.user %}
      <a href="{% url 'articles:update' article.pk %}">UPDATE</a><br>
      <form action="{% url 'articles:delete' article.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="DELETE">
      </form>
    {% endif %}
  ...
  ```

<br>

---

<br>

## User - Comment

```python
# articles/models.py

class Comment(models.Model):
		article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
		...
```

```bash
$ python manage.py makemigrations

# 첫번째 상황(null 값이 허용되지 않는 user_id 가 아무 값도 없이 comment 에 추가되려 하기 때문)
You are trying to add a non-nullable field 'user' to comment without a default; we can't do that (the database needs something to populate existing rows).
Please select a fix:
 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
 2) Quit, and let me add a default in models.py
Select an option: # 1 입력하고 enter

# 두번째 상황(그럼 기존 comment 의 user_id 로 뭘 넣을건지 물어봄, 현재 admin 인 1을 넣자)
Please enter the default value now, as valid Python
The datetime and django.utils.timezone modules are available, so you can do e.g. timezone.now
Type 'exit' to exit this prompt
>>> # 1 입력하고 enter (모든 댓글의 작성자를 admin 으로 하게 됨)

Migrations for 'articles':
  articles/migrations/0005_comment_user.py
    - Add field user to comment
```

```bash
$ python manage.py migrate
```

<br>

### CREATE & READ

- 해당 view 함수를 요청한 유저의 정보를 넣고나서 저장한다. (로그인 사용자만 작성하도록)

  ```python
  # articles/views.py
  
  @require_POST
  def comments_create(request, pk):
      if request.user.is_authenticated:
          article = get_object_or_404(Article, pk=pk)
          comment_form = CommentForm(request.POST)
          if comment_form.is_valid():
              comment = comment_form.save(commit=False)
              comment.article = article
              comment.user = request.user
              comment.save()
              return redirect('articles:detail', article.pk)
      return redirect('accounts:login')
  ```

- 비로그인 유저는 댓글 작성 form 을 볼 수 없도록 한다.

  ```django
  <!-- articles/detail.html -->
  
  {% extends 'articles/base.html' %}
  {% block content %}
    ...
  <hr>
    {% if request.user.is_authenticated %}
      <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
        {% csrf_token %}
        {{ comment_form }}
        <input type="submit">
      </form>
    {% else %}
      <a href="{% url 'accounts:login' %}">[댓글을 작성하려면 로그인하세요.]</a>
    {% endif %}
    <a href="{% url 'articles:index' %}">[back]</a>
  {% endblock %}
  ```

  ```python
  # articles/forms.py
  
  class CommentForm(forms.ModelForm):
  
      class Meta:
          model = Comment
          exclude = ['article', 'user',]
  ```

- 댓글 작성자 출력

  ```django
  <!-- articles/detail.html -->
  
  {{ comment.user }} - {{ comment.content }}
  ```

<br>

### DELETE

- 본인이 작성한 댓글만 삭제할 수 있어야 한다.

  ```python
  # articles/views.py
  
  @require_POST
  def comments_delete(request, article_pk, comment_pk):
      if request.user.is_authenticated:
          comment = get_object_or_404(Comment, pk=comment_pk)
          if request.user == comment.user:
              comment.delete()
      return redirect('articles:detail', article_pk)
  ```

  ```django
  <!-- articles/detail.html -->
  
  {% extends 'articles/base.html' %}
  {% block content %}
    ...
    <h4>댓글 목록</h4>
    {% if comments|length %}
      <p><b>{{ comments|length }}개의 댓글이 있습니다.</b></p>
    {% endif %}
    {% for comment in comments %}
      <div>
        {{ comment.user }} - {{ comment.content }}
        {% if request.user == comment.user %}
          <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method="POST" class="d-inline">
            {% csrf_token %}
            <input type="submit" value="DELETE">
          </form>
        {% endif %}
      </div>
    {% empty %}
      <p><b>댓글이 없어요..</b></p>
    {% endfor %}
    <hr>
    ...
  {% endblock %}
  ```

  