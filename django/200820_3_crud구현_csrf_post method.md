### CRUD

#### Read

* DB에 저장된 전체 글 목록을 가져와서 page에 보여주자.
* Article.objects.all()의 QuerySet을 그대로 context에 담아서 template 파일에 전달
* template은 for 문으로 하나씩 db값을 접근 가능하고 `.`연산자를 이용해서 값에 접근도 가능.



#### Create

* 생성하기 위한 경로하나 만들자(urls.py에 적어주기)

  > path("create/", views.create, name="create"),

* form 태그에서 데이터를 전달하고 그 데이터를 3가지 방법 중 하나로 DB에 저장하면 끝.

* GET/POST

  * GET: 주소줄에 쿼리 소트링 형식으로 데이터가 전달된다. 전달하는 길이에 한계가 있음(255자)

    * 주로 데이터 정보를 가져올 때 사용(즉, 데이터를 조회할 때 이용한다.)

  * POST: 패킷 바디 안에 데이터가 전달된다. 좀 더 많은 양의 데이터를 전달할 수 있음

    * 주로 데이터의 정보를 생성, 수정, 삭제할 때 사용한다. 

      

  * GET/article/: article의 정보를 조회하겠다.

  * POST/article/: article을 생성하겠다.

  * POST/article/3/: article을 수정하겠다.

  * REST API -> 면접에 자주 나옴. 



### csrf

사이트 간 요청 위조(또는 크로스 사이트 요청 위조, 영어: Cross-site request forgery, **CSRF**, XSRF)

내가 작성한 글인지 확실한 경우에만 데이터베이스에 저장한다.

```python
settings.py

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',  <=요거
```



crsf token을 보고 장고가 본인이 직접 작성한 내용인지 위조된 내용인지 확인한다.

데이터를 변경하는, POST방식일 때만 한다.

(GET은 공격해봤자 정보 조회만 가능하니까.)



* method를 POST로 변경할 때 해야할 일

GET에서 POST로 바뀌면 딱 2개만 다름.

`{% csrf_token %}` 이거 달아주고.

데이터받아올 때 request.GET을 `request.POST`로 변경한다.





```html
create.html

{% extends 'base.html' %}
{% block content %}
<form action="{% url 'articles:new' %}" method="POST">
  {% csrf_token %}
  title: <input type="text" name="title">
  <br>
  content : <Textarea name="content"></Textarea>
  <br>
  <button>제출</button>
</form>
{% endblock %}


이거 안하면 
Forbidden (403)
CSRF 검증에 실패했습니다. 요청을 중단하였습니다.
오류남.
```

`<form action="{% url 'articles:new' %}" method="POST">`이거

`<form action="/articles/new/"  method="POST">`로 해도되는데 그러면 나중에 다 수정해줘야되니까.



```python
IntegrityError at /articles/new/
NOT NULL constraint failed: articles_article.title
이런 오류 나는 이유
-> get방식이어서.!

POST로 바꿔주자.
viws.py
def new(request):

    # 세번째 방법
    title = request.POST.get('title')
    content = request.POST.get('content')

    Article.objects.create(title=title, content=content)

    return render(request, 'articles/new.html')
```



create하고 제출하면 다시 index화면으로 가고 싶을 때

urls.py

```python
from django.shortcuts import render, redirect
    return redirect('articles:index') 
-> 여기에 직접 return redirect('/articles/index/')로 해줘도 되는데 그러면 한가지 수정 이루어지면 다 신경써서 바꿔야하니까. 이름공간을 이용해서 적어줬다.

```

`return redirect('articles:index')  `

-> 이거 뜻: \`127.0.0.1:8000/articles/index로 가세요.` (그래서 def index 여기로 간다.)

```python
def index(request):

    articles = Article.objects.all()
    context = {
        'articles': articles,
    }

    return render(request, 'articles/index.html', context)

```

Redirect(): 이미 만들어진 페이지로 경로 재설정.



#### Update

* 글 제목을 클릭했을 때 해당 글만 보여지는 페이지를 만들면 된다.
* detail 페이지를 먼저 만들자!
  * 어떠한 글의 detail 페이지인지 해당 글의 정보(pk가 필요함.)
  * 글의 정보를 동적라우팅 방법으로 주소로 전달.
  * 주소로 전달받은 글의 pk값을 가지고 DB에서 데이터를 가져옴.
  * 가져온 데이터를 template 파일에서 보여주면 그것이 바로 detail page이다. 
* detail 페이지 하단에 수정하기 링크를 만들어준다.
  * 수정하기 링크는 edit하는 페이지를 보여주면 된다.
  * create 방법과 유사하게 from을 보여주는데, 
  * `해당 글의 data를 넘겨주고` 그 데이터를 `같이 보여주는게` 차이점이다.
  * 수정하기 최종버튼을 눌렀으면 POST방식으로 DB에 적용을 시켜주면 끝
  * 이 때 필요한 정보도 주소줄을 이용해서 전달한다.
* DB에 적용시키는 방법은,
  * 해당 pk를 가지는 데이터를 불러오고
  * 값을 수정하고
  * save한다.
* 끝나면 detail page로 redirect 시키면 된다.



하드코딩하지말자.

index.html

```html
<p>글 제목:  <a href="/articles/{{ article.pk }}/">{{ article.title }}</a></p>

```

```html
<p>글 제목:  <a href="{% url 'articles:detail' article.pk %}">{{ article.title }}</a></p>
```

  \#return redirect(f'/articles/{edit_data.pk}/')

  return redirect('article:detail', edit_data.pk) #그냥 pk해도됨. 둘다 같은 정보니까.







#### DELETE

* 삭제하기 버튼을 누르면, 삭제할 글의 pk가 같이 주소로 전달되고, 
* views.py에서 해당 pk값의 정보를 가져온 다음 delete함수를 호출하면 끝.!