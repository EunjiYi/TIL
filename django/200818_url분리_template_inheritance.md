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

> 각 app 폴더에 urls.py를 각각 작성한다. 

`include()`

- 다른 URLconf(app1/urls.py)들을 참조할 수 있도록 도와준다.
- Django가 함수 `include()`를 만나게 되면, URL의 그 시점까지 일치하는 부분을 잘라내고, 남은 문자열 부분을 후속 처리를 위해 include 된 URLconf로 전달한다.





```python
# urls.py

from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
]
```





---



## URL Name

> path() 함수의 name value를 작성해 `{% url %}` template tag로 호출

urls.py 에 app_name 을 통해 app 의 이름공간을 설정한다.

```python
# articles/views.py 

return render(request, 'articles/index.html')
```





## Template Inheritance

- **템플릿 상속을 위한 기본 세팅**
  - 프로젝트 폴더에서 `templates` 폴더 만든 후에 `base.html` 파일 생성한다.

> Django는 기본적으로 `app_name/templates` 를 바라보게 설정되어있다. (`APP_DIRS=True` 설정) 우리가 옮긴 위치는 `project폴더/templates` 이므로, Django는 현재 상태에서 해당 template 파일을 찾을 수 없다.



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





**`extends` tag**

> 이(자식) 템플릿이 부모 템플릿을 확장한다는 것을 알림

- `{% extends '' %}` 는 반드시 문서의 최상단에 위치해야 한다.

  ```django
  {% extends 'base.html' %}
  
  {% block content %}
    <h1>안녕하세요! 반갑습니다!!</h1>
  {% endblock %}
  ```



**`block` tag**

- 하위 템플릿에서 재 지정(overriden)할 수있는 블록을 정의
- 하위 템플릿이 채울 수 있는 공간



-------------------



**django 설계 철학 (Template)**

- 표현과 로직(view)을 분리

- 중복을 배제

   `템플릿 상속`의 기초가 되는 철학