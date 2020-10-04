[TOC]

# 08_django_many_to_many

> 2020.09.28

<br>

## Many-to-many relationship

> https://docs.djangoproject.com/en/3.1/ref/models/fields/#manytomanyfield

<br>

------

<br>

## ManyToManyField

> https://docs.djangoproject.com/en/3.1/ref/models/fields/#manytomanyfield

<br>

**개념**

- M:N(이하 다대다) 관계를 나타내기 위해 사용하는 필드

- 하나의 필수 위치인자(다대다 관계로 설정할 모델 클래스)가 필요하다.

<br>

**데이터베이스에서의 표현**

- django는 다대다 관계를 나타내는 중개 조인 테이블(intermediary join table)을 만든다.
- 테이블 이름은 ManyToManyField의 이름과 이를 포함하는 모델의 이름을 조합하여 생성한다.
- `db_table` 옵션을 사용하여 조인 테이블의 이름을 변경할 수도 있다.

<br>

**Arguments**

- `related_name`

  - ForeignKey의 related_name과 동일

- `through`

  - django는 다대다 관계를 관리하는 중개 테이블을 자동으로 생성한다.
  - 하지만, 중간 테이블을 직접 지정하려면 through 옵션을 사용하여 중개 테이블을 나타내는 Django 모델을 지정할 수 있다.
  - 일반적으로 추가 데이터를 다대다 관계와 연결하려는 경우에 사용한다.

- `symmetrical`

  - ManyToManyField가 동일한 모델을 가리키는 정의에서만 사용

  ```python
  from django.db import models
  
  class Person(models.Model):
      friends = models.ManyToManyField('self')
  ```

  - 예시처럼 동일한 모델을 가리키는 정의의 경우 django는 Person 클래스에 person_set 매니저를 추가 하지 않는다.
  - 대신 대칭적(`symmetrical`)이라고 간주하며, source 인스턴스가 target 인스턴스를 참조하면 target 인스턴스도 source 인스턴스를 참조하게 된다.
  - 즉, 내가 당신의 친구라면 당신도 내 친구가 된다.
  - self와의 관계에서 대칭을 원하지 않는 경우 `symmetrical=False`로 설정한다.

<br>

**중개 테이블 필드 생성 규칙**

1. 소스(source model) 및 대상(target model) 모델이 다른 경우
   - id
   - `<containing_model>_id`
   - `<other_model>_id`
2. ManyToManyField가 동일한 모델을 가리키는 경우
   - id
   - `from_<model>_id`
   - `to_<model>_id`

<br>

------

<br>

## LIKE

> `user` 는 여러 `article` 에 **좋아요를 누를 수 있고**
>
> `article` 은 여러 `user` 로부터 **좋아요를 받을 수 있다.**

<br>

**model 설정**

```python
# articles/models.py

class Article(models.Model):
    ...
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

<br>

- **현 상황에서는 `related_name` 작성이 필수**
  - M:N 관계 설정 시에 `related_name` 이 없다면 자동으로 `.article_set` 매니저를 사용할 수 있도록 하는 데 이 매니저는 이미 이전 1:N(User:Article) 관계에서 사용 중인 매니저이다.
  - user가 작성한 글들(`user.article_set`)과 user가 좋아요를 누른 글(`user.article_set`)을 django는 구분할 수 없게 된다.
    - `user.article_set.all()` : 유저가 작성한 게시글 전부
    - `user.like_articles.all()` : 유저가 좋아요를 누른 게시글 전부

<br>

- 여기서 `articles_article_like_users` 는 두 테이블 간의 M:N 관계를 나타내주는 중개 모델(Intermediary join table)이 된다.

  ```sql
  $ sqlite3 db.sqlite3
  
  sqlite> .tables
  articles_article             auth_user_groups
  articles_article_like_users  auth_user_user_permissions
  articles_comment             django_admin_log
  auth_group                   django_content_type
  auth_group_permissions       django_migrations
  auth_permission              django_session
  auth_user
  ```

<br>

- **이제 사용 가능한 ORM 명령어는 다음과 같다.**
  - `article.user` : 게시글을 작성한 유저 - 1:N
  - `article.like_users` : 게시글을 좋아요한 유저 - M:N
  - `user.article_set`: 유저가 작성한 게시글들 → 역참조 - 1:N
  - `user.like_articles`: 유저가 좋아요한 게시글들 → 역참조 - M:N

<br>

**view & urls**

> https://docs.djangoproject.com/en/3.1/ref/models/querysets/#filter
>
> https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet.exists

- `filter()` 
  - 조건에 맞는 여러 행을 출력한다. (조건에만 맞는다면 전부 가져온다.)
- `exists()`
  - 최소한 하나의 레코드가 존재하는지 여부를 확인하여 알려 준다. 

- `.get` 이 아닌 `.filter` 를 사용하는 이유 → 데이터가 없는 경우의 오류 여부
  - `.get()` 은 유일한 값을 꺼낼 때 사용한다.(ex. pk) 유일한 값을 꺼낸다는 것은 해당 데이터가 존재하지 않는 경우가 없다는 뜻이다. 값이 없으면 에러(DoesNoetExist error) 가 발생하기 때문에 무조건 존재하는 값에 접근할 때 사용한다.
  - `.filter()` 의 경우 조건에 맞는 여러 개의 데이터를 가져온다. 이때 데이터가 1개도 없어도 빈 쿼리셋을 반환한다. (몇 개인지 보장할 수 없을 때)

```python
# articles/urls.py

urlpatterns = [
		...
		path('<int:article_pk>/like/', views.like, name='like'),
]
```

```python
# articles/views.py

@require_POST
def like(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)
        user = request.user

        if article.like_users.filter(pk=user.pk).exists():
        # if user in article.like_users.all():
            article.like_users.remove(user)
        else:
            article.like_users.add(user)
        return redirect('articles:index')
    return redirect('accounts:login')
```

<br>

- **font awesome 을 활용한 like 버튼 만들기**

> https://fontawesome.com/

```html
<!-- articles/base.html -->

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="<https://kit.fontawesome.com/dacaadcd9c.js>" crossorigin="anonymous"></script>
...
```

```django
<!-- articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">Articles</h1>
  ...
    <p>글 내용: {{ article.content }}</p>
    <form action="{% url 'articles:like' article.pk %}" method="POST" class="d-inline">
      {% csrf_token %}
      {% if user in article.like_users.all %}
        <button class="btn btn-link">
          <i class="fas fa-heart fa-lg" style="color:crimson;"></i>
        </button>
      {% else %}
        <button class="btn btn-link">
          <i class="fas fa-heart fa-lg" style="color:black;"></i>
        </button>
      {% endif %}
    </form>
    {{ article.like_users.all|length }} 명이 이 글을 좋아합니다.<br>
    <a href="{% url 'articles:detail' article.pk %}">[detail]</a>
    <hr>
  {% endfor %}
{% endblock %}
```

<br>

## Profile

```python
# accounts/urls.py

path('<username>/', views.profile, name='profile'),
```

```python
# accounts/views.py

def profile(request, username):
    person = get_object_or_404(get_user_model(), username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```

```django
<!-- accounts/profile.html -->

{% extends 'base.html' %}

{% block content %}
<h1 class="text-center">{{ person.username }}님의 프로필</h1>

<!-- bootstrap 점보트론 들어갈 곳 -->

<hr>

<h2>{{ person.username }}가 쓴 글</h2>
{% for article in person.article_set.all %}
  <div>{{ article.title }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}가 쓴 댓글</h2>
{% for comment in person.comment_set.all %}
  <div>{{ comment.content }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}가 좋아요한 글</h2>
{% for article in person.like_articles.all %}
  <div>{{ article.title }}</div>
{% endfor %}

<hr>

<a href="{% url 'articles:index' %}">back</a>
{% endblock  %}
```

```django
<!-- base.html -->

<a href="{% url 'accounts:profile' request.user.username %}">내프로필</a>
```

```django
<!-- articles/index.html -->

<p><b>작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></b></p>
```



<br>

## FOLLOW

**models**

```python
# accounts/models.py

from django.conf import settings

class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

<br>

**Follow 구현**

> 자기자신은 follow 하면 안된다.

```python
# accounts/urls.py

urlpatterns = [
    ...,
    path('follow/<int:user_pk>/', views.follow, name='follow'),
]
```

```python
# accounts/views.py

@require_POST
def follow(request, user_pk):
    you = get_object_or_404(get_user_model(), pk=user_pk)
    me = request.user

    if me != you:
        # if user in person.followers.all():
        if you.followers.filter(pk=me.pk).exists():
            you.followers.remove(me)
        else:
            you.followers.add(me)
    return redirect('accounts:profile', you.username)

```

<br>

**templates**

- 템플릿 분할

   로 분할

```django
<!-- accounts/_follow.html -->

<!-- 팔로워 수 / 팔로잉 수 -->
<div class="jumbotron">
  <p class="lead">
    팔로워 수 : {{ person.followers.all|length }} / 팔로잉 수 : {{ person.followings.all|length }} 
  </p>
  <!-- 팔로우 버튼 / 언팔로우 버튼 -->
  {% if request.user != person %}
    <form action="{% url 'accounts:follow' person.pk %}" method="POST">
      {% csrf_token %}
      {% if request.user in person.followers.all %}
      <button class="btn btn-secondary">Unfollow</button>
      {% else %}
      <button class="btn btn-primary">Follow</button>
      {% endif %}
    </form>
  {% endif %}
</div>
```

```django
<!-- articles/detail.html -->

{% extends 'base.html' %}

{% block content %}
<h1 class="text-center">{{ person.username }}님의 프로필</h1>

{% include 'accounts/_follow.html' %}

<hr>
```

<br>

**`with` template tag**

>  https://docs.djangoproject.com/en/3.1/ref/templates/builtins/#with

- 더 간단한 이름으로 복잡한 변수를 저장한다.
- 주로 데이터베이스에 중복으로 여러번 엑세스 할 때 유용하게 사용한다.
- 변수는 `{% with %}` and `{% endwith %}` 사이에서만 사용 가능하다.

```django
<!-- accounts/_follow.html -->

<!-- 팔로워 수 / 팔로잉 수 -->
<div class="jumbotron">
  {% with followers=person.followers.all followings=person.followings.all %}
    <p class="lead">
      팔로워 수 : {{ followers|length }} / 팔로잉 수 : {{ followings|length }} 
    </p>
    <!-- 팔로우 버튼 / 언팔로우 버튼 -->
    {% if request.user != person %}
      <form action="{% url 'accounts:follow' person.pk %}" method="POST">
        {% csrf_token %}
        {% if request.user in followers %}
          <button class="btn btn-secondary">Unfollow</button>
        {% else %}
          <button class="btn btn-primary">Follow</button>
        {% endif %}
      </form>
    {% endif %}
  {% endwith %}
</div>
```

------