[TOC]

# 01_django_model

> 2020.08.19

## Model Intro

**Model 개념**

- 모델은 단일한 **데이터에 대한 정보**를 가지고 있다.
- 필수적인 필드(컬럼)와 데이터(레코드)에 대한 정보를 포함한다.
- 일반적으로 각각의 **모델(클래스)**는 단일한 데이터베이스 **테이블과 매핑**된다.
- 모델은 부가적인 메타데이터를 가진 **DB의 구조(layout)를 의미**
- 사용자가 저장하는 **데이터들의 필수적인 필드와 동작(behavior)** 포함



**DB의 기본 구조**

- 데이터베이스 (DB)

  - 체계화된 데이터의 모임

- 쿼리(Query)

  - 데이터를 조회하기 위한 명령어
  - (주로 테이블형 자료구조에서) 조건에 맞는 데이터를 추출하거나 조작하는 명령어
  - Query를 날린다 → 데이터를 DB에 요청 → 응답 데이터는 QuerySet (Model의 인스턴스)

- 스키마 (Schema) / 뼈대(Structure)

  - 데이터베이스에서 자료의 구조, 표현 방법, 관계 등을 정의한 구조
  - 데이터베이스 관리 시스템(DBMS)이 주어진 설정에 따라 데이터베이스 스키마를 생상하며, 데이터베이스 사용자가 자료를 저장, 조회, 삭제, 변경할 때 DBMS는 자신이 생성한 데이터베이스 스키마를 참조하여 명령을 수행

- 테이블 (Table) / 관계(Relation)

  - 필드(field): 속성, 컬럼(Column)

    - 모델 안에 정의한 클래스에서 클래스 변수가 필드가 된다.

  - 레코드(record): 튜플, 행(Row)

    - 우리가 ORM을 통해 해당하는 필드에 넣은 데이터(값)을 의미한다.

    

------



## ORM

**개념**

- "Object-Relational-Mapping 은 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템간에(Django - SQL)데이터를 변환하는 프로그래밍 기술이다. 이것은 프로그래밍 언어에서 사용할 수 있는 '가상 객체 데이터베이스'를 만들어 사용한다."

- OOP 프로그래밍에서 RDBMS을 연동할 때, 데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법이다. 객체 관계 매핑이라고도 부른다.
- 객체 지향 언어에서 사용할 수 있는 '가상' 객체 데이터베이스를 구축하는 방법이다.
- 현대 대부분의 프레임워크는 ORM 사용



**장/단점**

- 장점
  - SQL을 몰라도 DB 연동이 가능하다. (SQL 문법을 몰라도 쿼리 조작 가능)
  - SQL의 절차적인 접근이 아닌 객체 지향적인 접근으로 인해 `생산성`이 증가한다.
  - ORM은 독립적으로 작성되어 있고, 해당 객체들을 재활용할 수 있다. 때문에 모델에서 가공된 데이터를 컨트롤러(view)에 의해 뷰(template)과 합쳐지는 형태로 디자인 패턴을 견고하게 다지는데 유리
- 단점
  - ORM 만으로 완전한 서비스를 구현하기 어렵다.
  - 프로젝트의 복잡성이 커질 경우 설계 난이도가 상승할 수 있다.



**정리**

- 객체 지향 프로그래밍에서 DB를 편리하게 관리하게 위해 ORM 프레임워크를 도입
- **"우리는 DB를 객체(object)로 조작하기 위해 ORM을 사용한다."**



------



## Model

**프로젝트 시작**

> 01_django_model 폴더 안에서 진행

```bash
$ django-admin startproject crud 
$ cd crud
$ python manage.py startapp articles
```

```python
# crud/settings.py

INSTALLED_APPS = [
    'articles',	
		...
]
```



**models.py 정의**

```python
# articles/models.py

class Article(models.Model): # 상속
    # id는 기본적으로 처음 테이블 생성시 자동으로 만들어진다.
    title = models.CharField(max_length=10) # 클래스 변수(DB의 필드)
    content = models.TextField() 
    created_at = models.DateTimeField(auto_now_add=True)
```



**대표 필드**

- `CharField(max_length=None, **options)`

  - 길이의 제한이 있는 문자열을 넣을 때 사용
  - CharField의 max_length는 필수 인자
  - **필드의 최대 길이(문자),** 데이터베이스 레벨과 Django의 유효성 검사(값을 검증하는 것)에서 활용
  - 문자열 필드 → 텍스트 양이 많을 경우 `TextField()` 사용
  - 기본 양식 위젯은 **TextInput**

- `TextField(**options)`

  - **글의 수가 많을 때 사용**
  - max_length 옵션을 주면 자동양식필드의 textarea 위젯에 반영은 되지만 모델과 데이터베이스 수준에는 적용되지 않는다. (CharField 를 사용)
  - 기본 양식 위젯은 **Textarea**

- `DateTimeField(auto_now=False, auto_now_add=False, **options)`

  - 최초 생성 일자

    - `auto_now_add=True`

    - django ORM이 **최초 insert(테이블에 데이터 입력)시**에만 **현재 날짜와 시간으로 갱신(테이블에 어떤 값을 최초로 넣을 때)**

  - 최종 수정 일자

    - `auto_now=True`

    - django ORM이 **save를 할 때마다 현재 날짜와 시간으로 갱신**

    

------



## Migrations

>  django가 모델에 생긴 변화(필드를 추가했다던가 모델을 삭제했다던가 등)를 반영하는 방법



**makemigrations**

> migration 파일은 데이터베이스 스키마를 위한 버전관리 시스템이라 생각하자

- 모델을 변경한 것에 기반한 새로운 마이그레이션을 만들 때 사용
- 모델을 활성화 하기 전에 DB 설계도(마이그레이션) 작성
- migrations 폴더 안에 정의한 class를 토대로 Django ORM이 우리에게 만들어준 설계도 확인

```bash
$ python manage.py makemigrations
```

- `0001_initial.py` 생성 확인

```python
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

```bash
$ python manage.py makemigrations
```



**migrate**

> 설계도를 실제 DB에 반영하는 과정

- 기존에 내가 `[models.py](<http://models.py>)` 에 정의하지 않았던 테이블은 장고가 기존에 미리 만들어 놓은 테이블

- `migrate` 는 `makemigrations` 로 만든 설계도를 실제 `db.sqlite3` DB에 반영한다.

- 모델에서의 변경 사항들과 DB의 스키마가 동기화를 이룬다.

  ```bash
  $ python manage.py migrate
  ```



**sqlmigrate**

- 해당 migrations 설계도가 SQL 문으로 어떻게 해석되어서 동작할지 미리 확인 할 수 있다.

  ```bash
  $ python manage.py sqlmigrate app_name 0001
  ```



**showmigrations**

- migrations 설계도들이 migrate 됐는지 안됐는지 여부를 확인 할 수 있다.

  ```bash
  $ python manage.py showmigrations
  ```

  

**Model 중요 3단계**

- `models.py` : 변경사항 발생 (생성 / 수정)
- `makemigrations` : migration 파일 만들기 (설계도)
- `migrate` : DB에 적용 (테이블 생성)



**실제 DB 테이블 확인**

- vs code extension - sqlite3 검색 후 설치

  - `F1` → `sqlite` 검색 후 Open Database 클릭 → `db.sqlite3` 클릭 → 왼쪽 EXPLORER 하단에 보면 `SQLITE EXPLORER` 라고 되어있는 거 클릭

- 테이블을 확인해보면 `articles_article`이라는 이름으로 테이블 생성

  - INSTALLED_APPS 중 몇몇은 최소 하나 이상의 DB 테이블을 사용하기 때문에 migrate 와 함께 테이블이 만들어진다.

  - 테이블의 이름은 app의 이름과 model 의 이름이 조합(모두 소문자)되어 자동으로 생성된다. (소문자)

    - `app이름(articles)_소문자 모델이름(article)`

    

------



## Database API

> django가 기본적으로 orm을 제공함에 따른 것으로 db를 편하게 조작할 수 있도록 도와줌



**Django shell**

- 일반 파이썬 쉘을 통해서는 장고 프로젝트 환경에 접근할 수 없음

- 그래서 장고 프로젝트 설정이 로딩된 파이썬 쉘을 활용

- Django 프로젝트 내의 각종 모듈 패키지를 활용 할 수 있다.

  ```bash
  $ pip install ipython django-extensions
  ```

  ```python
  # settings.py
  
  INSTALLED_APPS = [
      ...
      'django_extensions',
      ...
  ]
  ```

  ```bash
  $ python manage.py shell_plus
  ```



**DB API 구문**

> https://docs.djangoproject.com/en/3.1/ref/models/querysets/#queryset-api-reference
>
> https://docs.djangoproject.com/en/3.1/topics/db/queries/#making-queries

<img width="1517" alt="Screen Shot 2020-08-19 at 5 12 04 PM" src="https://user-images.githubusercontent.com/18046097/90609518-23395b80-e23f-11ea-8e04-abfc04a709f7.png">





**`objects` Manager**

> https://docs.djangoproject.com/en/3.1/topics/db/managers/#managers
>
> Django 모델에 데이터베이스 쿼리 작업이 제공되는 인터페이스

- Model Manager와 Django Model 사이의 ***Query 연산의 인터페이스 역할\*** 을 해주는 친구
- 즉, `models.py` 에 설정한 클래스(테이블)을 불러와서 사용할 때 DB와의 interface 역할을 하는 매니저
- ORM이 클래스로 만든 인스턴스 객체와 db를 연결하는데 그 사이에서 통역 역할을 하는게 ORM의 역할이라고 생각하면 된다. 즉, DB를 Python class로 조작할 수 있는 manager
  - **Python Class(`models.py`) ——— objects ——— DB**
- Django는 기본적으로 모든 Django 모델 클래스에 대해 '`objects`' 라는 Manager(django.db.models.Manager) 객체를 자동으로 추가한다.
- Manager(objects)를 통해 특정 데이터를 조작(메서드)할 수 있다.



**QuerySet**

> 데이터베이스로부터 데이터를 읽고, 필터를 걸거나 정렬 등을 수행
>
> 쿼리(질문)를 DB에게 던져서 글을 읽거나, 생성하거나, 수정하거나, 삭제

- 데이터베이스에서 전달 받은 객체의 목록
- django orm에서 발생한 자료형
- objects를 사용하여 복수의 데이터를 가져오는 함수를 사용할 때 반환되는 객체
- 단일한 객체를 리턴할 때는 테이블(Class)의 인스턴스로 리턴됨



---



# **CRUD**

> 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 
> Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말
>
> 이러한 4개의 조작을 모두 할 수 없다면 그 소프트웨어는 완전하다고 할 수 없다. 
>
> 이들 기능은 매우 기본적이기 때문에, 한 묶음으로 설명되는 경우가 많다.
>
> https://ko.wikipedia.org/wiki/CRUD



## Create

**기초 설정**

- `articles_article` 테이블을 사용하기 위해서는 `import` 가 필요하다.

```python
# DB에 앤스턴스 객체를 얻기 위한 쿼리문 날리기
>>> Article.objects.all()
<QuerySet []>
```



**데이터 객체를 만드는(생성하는) 3가지 방법**

**첫번째 방식**

- ORM을 쓰는 이유는 DB 조작을 객체 지향 프로그래밍(클래스)처럼 하기 위해
  - `article = Article()` :  클래스로부터 인스턴스 생성
  - `article.title` : 인스턴스로 클래스 변수에 접근해 해당 인스턴스 변수를 변경
  - `article.save()` : 인스턴스로 메소드를 호출

```python
>>> article = Article() # Article(class)로부터 article(instance)
>>> article
<Article: Article object (None)>

>>> article.title = 'first' # 인스턴스 변수(title)에 값을 할당
>>> article.content = 'django!' # 인스턴스 변수(content)에 값을 할당

# save 를 하지 않으면 아직 DB에 값이 저장되지 않음
>>> article
<Article: Article object (None)>

>>> Article.objects.all()                            
<QuerySet []>

# save 를 하고 확인하면 저장된 것을 확인할 수 있다
>>> article.save()
>>> article
<Article: Article object (1)>
>>> Article.objects.all()
<QuerySet [Article: Article object (1)]>

# 인스턴스인 article을 활용하여 변수에 접근해보자(저장된걸 확인)
>>> article.title
'first'
>>> article.content
'django!'
>>> article.created_at
datetime.datetime(2019, 8, 21, 2, 43, 56, 49345, tzinfo=<UTC>)
```



**두번째 방식**

- 함수에서 keyword 인자를 넘기는 방식과 동일

```python
>>> article = Article(title='second', content='django!!')

# 아직 저장이 안되어 있음
>>> article
<Article: Article object (None)>

# save를 해주면 저장이 된다.
>>> article.save()
>>> article
<Article: Article object (2)>
>>> Article.objects.all()
<QuerySet [<Article: Article object (1)>, <Article: Article object (2)>]>

# 값을 확인해보자
>>> article.pk
2
>>> article.title
'second'
>>> article.content
'django!'
```



**세번째 방식**

- `create()` 를 사용하면 쿼리셋 객체를 생성하고 저장하는 로직이 한번의 스텝으로 가능

```python
>>> Article.objects.create(title='third', content='django!')
<Article: Article object (3)>
**__str__**
```



`__str__`

- 모든 모델마다 표준 파이썬 클래스의 메소드인 **str**() 을 정의하여 각각의 object가 사람이 읽을 수 있는 문자열을 반환(return)하도록 한다.

  ```python
  class Article(models.Model):
      title = models.CharField(max_length=10)
      content = models.TextField()
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
  
      def __str__(self):
          return self.title
  ```



**`save()`**

> https://docs.djangoproject.com/en/3.1/ref/models/instances/#saving-objects

- `.save()` 메서드 호출을 통해 데이터를 DB에 저장한다.



## Read

`all()`

> https://docs.djangoproject.com/en/3.1/ref/models/querysets/#all

- `QuerySet` return
- 리스트는 아니지만 리스트와 거의 비슷하게 동작

```python
>>> Article.objects.all()
<QuerySet [<Article: Article object (1)>, <Article: Article object (2)>, <Article: Article object (3)>, <Article: Article object (4)>]>
get()
```



`get()`

>  https://docs.djangoproject.com/en/3.1/ref/models/querysets/#get

- 객체가 없으면 `DoesNotExist` 에러가 나오고 객체가 여러 개일 경우에 `MultipleObjectReturned` 오류를 띄움.
- 위와 같은 특징을 가지고 있기 때문에 unique 혹은 Not Null 특징을 가지고 있으면 사용할 수 있다.

```python
>>> article = Article.objects.get(pk=100)
DoesNotExist: Article matching query does not exist.

>>> Article.objects.get(content='django!')
MultipleObjectsReturned: get() returned more than one Article -- it returned 2!
**filter()**
```



`filter()`

>  https://docs.djangoproject.com/en/3.1/ref/models/querysets/#filter

- 지정된 조회 매개 변수와 일치하는 객체를 포함하는 새 QuerySet을 반환

  ```python
  >>> Article.objects.filter(content='django!')
  <QuerySet [<Article: first>, <Article: fourth>]>
  
  >>> Article.objects.filter(title='first')
  <QuerySet [<Article: first>]>
  ```



**The `pk` lookup shortcut**

> https://docs.djangoproject.com/en/3.1/topics/db/queries/#the-pk-lookup-shortcut

- 또한, 우리가 `.get(id=1)` 형태 뿐만 아니라 `.get(pk=1)` 로 사용할 수 있는 이유는(DB에는 id로 필드 이름이 지정 됨에도) `.get(pk=1)` 이`.get(id__exact=1)` 와 동일한 의미이기 때문이다. 

- pk는 `id__exact` 의 shortcut 이다.

  ```python
  >>> Blog.objects.get(id__exact=14) # Explicit form
  >>> Blog.objects.get(id=14) # __exact is implied
  >>> Blog.objects.get(pk=14) # pk implies id__exact
  ```



## Update

- article 인스턴스 객체 생성
- `article.title = 'byebye'` : article 인스턴스 객체의 인스턴스 변수에 접근하여 기존의 값을 `byebye` 로 변경
- `article.save()` : article 인스턴스를 활용하여 `save()` 메소드 실행

```python
# UPDATE articles SET title='byebye' WHERE id=1;
>>> article = Article.objects.get(pk=1)
>>> article.title
'first'

# 값을 변경하고 저장
>>> article.title = 'byebye'
>>> article.save()

# 정상적으로 변경된 것을 확인
>>> article.title
'byebye'
```



## Delete

- article 인스턴스 생성후 `.delete()` 메서드 호출

```python
>>> article = Article.objects.get(pk=1)

# 삭제
>>> article.delete()
(1, {'articles.Article': 1})

# 다시 1번 글을 찾으려고 하면 없다고 나온다.
>>> Article.objects.get(pk=1)
DoesNotExist: Article matching query does not exist.
```



---



# Admin

**개념**

- 사용자가 아닌 서버의 관리자가 활용하기 위한 페이지
-  Article class를 `admin.py` 에 등록하고 관리
- `django.contrib.auth` 모듈에서 제공하는 것 → Django에서 제공되는 Authentication 인증 프레임워크
- record 생성 여부 확인에 매우 유용하고 CRUD 로직을 확인하기에 편리하다.



**관리자 생성**

- 관리자 계정 생성 후 서버를 실행한 다음 `/admin` 으로 가서 관리자 페이지 로그인

- 계정만 만들면 실제로 Django 관리자 화면에서 아무 것도 보이지 않는다.

- `admin.py` 로 가서 관리자 사이트에 등록하여 내가 만든 record를 보기 위해서는 Django 서버에 등록

- 처음에 auth 관련된 기본 테이블이 생성되지 않으면 관리자를 생성할 수 없다.

  ```bash
  $ python manage.py createsuperuser
  ```



**admin 페이지 확인**

- admin 사이트에 방문해서 우리가 현재까지 작성한 글들을 확인 해보자.
- `admin.py` 는 관리자 사이트에 Article 객체가 관리 인터페이스를 가지고 있다는 것을 알려주는 것이다.
- 이렇게 admin 사이트에 등록된 모습이 어딘가 익숙하다? 바로 `models.py` 에 정의한 `__str__` 의 형태로 객체가 표현된다. (`list_display` 를 설정하지 않은 경우 1개의 column으로 표현됨)
- 실제로 `__str__` 부분을 주석처리 하고 reload를 하면 객체 자체가 출력 되는 것을 볼 수 있다.



**작성**

```python
# articles/admin.py

from django.contrib import admin
from .models import Article # 명시적 상대경로 표현

admin.site.register(Article)
```



------