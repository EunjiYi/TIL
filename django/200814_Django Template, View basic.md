### 파이썬 기반의 웹프레임워크 = django



풀스텍 프레임워크다. for 파이썬

cf. 플라스크는 마이크로프레임워크 - 작은거 빨리빨리 확인할 때 많이 씀.



material theme icons -> vs code extension 파일이랑 폴더 구분할 때 좋다

터미널창 clear = crtl+l



django설치

오픈제이슨(ctrl_shift+p > jsonr검색> Preferences:Open Settings(JSON)) 에 usage 복붙/추가하기

```
    //django
    "files.associations": {
        "**/*.html": "html",
        "**/templates/**/*.html": "django-html",
        "**/templates/**/*": "django-txt",
        "**/requirements{/**,*}.{txt,in}": "pip-requirements"
    },


    "emmet.includeLanguages": {"django-html": "html"}
```





> ## Django

Django 3.1

`pip list`

- 장고 설치하기

  ```django
  pip install django
  
  #특정 버전으로 받고 싶을 때
  pip install django==3.0.8
  ```
  



설치확인하기 pip list

Django       3.1



$ python -m django --version

3.1



- 장고 시작하기

  장고 프로젝트만들기

  $ django-admin startproject first_project

  프로젝트이름

  하이픈 사용 안됨.

  python이나 장고에서 쓰는 기본 이름들 안 됨. - 함수이름, 특정모듈이름 등

  ```django
  djang-admin startproject first_wbex
  ```

  - `first_webex`라는 이름으로 폴더가 생성 (가장 바깥에 있는 프로젝트 폴더명은 수정 가능하나, setting 파일이 들어있는 폴더명은 건드리지  말자.)

    - 여기 안에는 `first_webex` 폴더와 `manage.py` 생성되어짐

      - fisrt_webex: 프로젝트 설정 파일들이 들어있다. 

      - manage.py: 장고 명령어를 실행하기 위한 파일

        `python manage.py 장고 명령어`



cd first_project

서버켜기

python manage.py runserver



서버의 주소

Starting development server at http://127.0.0.1:8000/

내가 접속할수있는 localhost주소 여기에 장고가 켜져있다.



* 장고 실행하기

  ```
  python manage.py runserver
  ```

  

프로젝트는 어플리케이션들의 집합
$ python manage.py startapp

$ python manage.py startapp articles

앱의 이름은 복수형으로 쓰는게 권장사항

반드시 앱 설치하고 앱 등록하기!!



```
settings.py
장고 컨벤션
앱 반드시 위부터 작성
(1) local apss
(2) 3rd party apps
(3) django apps

리스트 마지막에 콤마있음 = 장고만의 문법 = 트뤠이링콤마 = 따라오는 콤마
바로 아래에 새로운거 추가하기 좋도록.
없어도 오류는 안나는데 있는데 컨벤션
딕셔너리 마지막 요소에도 트레이링 콤마 넣어준다.
```





- 장고 프로젝트는 Application의 집합체로 동작
  - 실제로 어떠한 역할을 해주는 친구가 바로 app.
  - 하나의 프로젝트는 여러 개의 어플리케이션을 가질 수 있음
    - 어플리케이션: 하나의 역할 및 기능 단위로 쪼개진 형태.
      - 회원관리/글 작성, 수정, 글 삭제/데이터를 수집분석/....
      - 어플리케이션을 이렇게 나뉘어야한다! 하는 기준은 없음
      - 작은 프로젝트라면 어플리케이션을 따로 나누지 않아도 된다.



- 어플리케이션 생성

  ```dj
  python manage.py startapp 앱이름(복수형이 컨벤션)
  전체적인 실행은 프로젝트에서 manage.py가 실행됨(settings를 기반으로.)그래서 app들을 셋팅해줘야한다.
  ```

  - 해당 앱이름으로 폴더가 생성됨(앱 폴더)
  - 바로 할일이 있다!!! (안하면 1000퍼센트로 까먹음)

  1. 앱생성
  2. 앱등록

  - `settings.py` 에 내가 생성한 app을 등록해줘야함
  - `installed.app에 가장 윗줄에 등록`
  - LANGUAGE_CODE = ``ko-kr`` 웬만하면 소문자로
  - TIME_ZONE = `Asia/Seoul` 앞에만 대문자!





- MTV(MVC패턴)
  
- ```
  MVC패턴(Django에서는 MTV)
  
  * Model - View - Controller
  * Model- Template - View
  ```
  
  -  Model: 장고에서는 Model
  -  View:  장고에서는 Templete
  - Controller:  장고에서는 View
  
- 뷰가 이름이 같아서 헷갈린다..... 조심
  
  
  
- **3대장: 우리가 가장 밀접하게 수정해야하는 파일명**

- 데이터흐름 그대로 코딩하자.

​	1. urls.py

	2. views.py
 	3. templates(html 들)



- url.py에서 해야할 일
  - **path('url 패턴/', 실행이 되어야 하는 views에 있는함수, 해당 path의 별명)**
    
  - 많이 놓치는 부분: `url 패턴 뒤에 슬러쉬!!`
    
    - 경로 없이 'index.html'만 써도 알아서 템블릿 폴더안에 있는걸 인식함 - 장고와의 약속을지켜서 폴더를 만들었기 때문이다.
    
  - url뒤쪽에 어팬드 슬래쉬 해야함 - 장고특성
  
    
  
- views.py에서 해야할 일

  - 함수를 정의(첫번째 인자로 request 필수!! 꼭!! 그냥필수!!)

  - return은 꼭 필요

    - **render: 주로 template과 함께 response할때 사용되는 함수**
  - rendering 뭘 만든다. 
    - 렌더함수(무조건 첫번째 요소 리쿼스트, 요청할 페이지)
    
  - 뷰 함수는 첫번째인자로 반드시 리퀘스트를 받아야한다.




* templates

  * 장고가 내부적으로 알고있는 경로로 맞춰서 템플릿 직접 만들기

  * articles(app폴더 안에) -> templates 폴더 만들기

    

많이 나는 에러 `TemplateDoesNotExist at /index/`

오타

오타

templates 폴더에 위치하지 않으면 

TemplateDoesNotExist 뜬다. 



- JSON 파일

  - 파이썬 딕셔너리와 구조가 똑같다.

    { key : value }

  - 단 차이점은 `json은 binary 형식으로 저장`된다. 

    - 쉽게 말하면 문자열로 저장이 된다.
    - 더 쉽게 말하면 10  vs '10'

    

---------

url을 변수화

### variable rounting

```
path('hello/<str:name>/')
```

```
#blank 2 lines by pep8
def dineer(request):
	menus = ['족발', '햄버거', '치킨', '초밥']
	pick1 = random.choice(menus)
	return render(request, 'dinner.html', {'pick': pick1,})

```

> key값으로 호출한다.즉 pick으로 pick1 데이터를 호출함.



```
https://search.naver.com/search.naver? (중략) query=헤이즈 //구글은 그냥 q
```

key = value

{ input name : input value }



render

주로 html파일과 같이 보여줄 때 rendor함수를 사용한다. 장고자체 기능





method 

get은 쿼리스트링으로 넘어간다.

key(input의 name) : value(input의 벨류) - 데이터를 조회할 때

post는 htttp의 body쪽으로 넘어감 - 데이터의 변경을 줄때





전체적인 절차

django 설치

django pjt 생성

서버를 켜서 로켓페이지 확인

> admin이랑 index, dinner는 잘 나오는데 그냥 127.0.0.1:8000화면이 안나오는데 이건 정상인가요? Page not found (404)
>
> 네네 application 을 등록하면 더이상 로켓은 볼 수 없구요밑에 등록된 url 경로로만 접근이 가능합니다

django app 생성

app을 pjt에 등록

urls -> views -> template





### Django Template Language(DTL)

django template에서 사용하는 built-in template system이다.

조건, 반복, 변수치환, 필터 등의 기능을 제공

프로그래밍적 로직이 아니라 프레젠테이션을 표현하기 위한 것

파이썬처럼 if, for를 사용할 수 있지만 `이것은 단순히 python code로 실행되는 것이 아니다.`



**syntax**

- variables  `{{ variable }}`
  - context에서 값을 출력하는데, context는 키를 값에 매핑하는 딕셔너리와 유사한 객체
- tags  `{% tag %}`
- filters  `{{ variable|filter }}`
  - 변수 및 태그 인수의 값을 변환
- comments



### 템플릿 시스템 설계 철학

- django는 템플릿 시스템이 `표현`을 제어하는 도구이자 `표현에 관련된 로직일 뿐`이라고 생각한다.
- 템플릿시스템에서는 이러한 기본 목표를 넘어서는 기능을 지원해서는 안된다. 즉 연산하면 안됨.





**Form**

- 웹에서 사용자 정보를 입력하는 여러(text, button, checkbox, file, hidden, image, password, radio, reset, submit, select, input) 방식을 제공하고, **사용자로부터 할당된 데이터를 서버로 전송하는 역할**을 담당하는 HTML 태그





# 장고 동작 정의 방법

* Template Variable

  * html과 같은 template에서 views.py에서 준비한 변수를 가져다 쓰기 위한 방법

  * render() 세번째 인자로 `{'key': value}`와 같이 딕셔너리 형태로 넘겨주면 template에서 key를 이용하여 value를 가져 올 수 있다.

    ```html
    context = {'key': value}
    return render(request, 'index.html', context)
    ```

    ```
    {{ key }}로 value를 보여줄 수 있다.
    ```

* Variable Routing(동적 라우팅)

  * url주소 일부를 변수처럼 사용해서 동적인 주소를 만드는 것

    주소 요청 : `https://127.0.0.1:8000/hello/문자열`

    urls.py

    ```
    path('hello/<str(타입):name(변수명)>/'. views.hello),
    ```

    * 타입을 int로 넣으면 숫자를 받을 수 있다.

    views.py

    ```
    def hello(request, name(변수명)):
    	print(name)
    	context {
    		'name': name
    	}
    	return render(request, 'hello.html', context)
    ```

    template(hello.html)

    ```
    <body>
    	<h1>이름은 : {{ name }}</h1> #context의 key값을 사용하면 value를 출력한다.
    </body>
    ```

    

* DTL (tag와 filter)

  * 로직을 표현 할 때는 : `{% for %}`

  * 값을 표현 할 때는 : `{{ }}`

  * 주석으로 나타내고 싶을 때는 : `{# #}`

  * 여러줄 주석 : `{% comment %} {% endcomment %}`

  * DTL을 사용할 때 html 주석(<!-- -->)을 사용하면 주석처리가 되지않아 오류나는 경우 발생

    ```
    <!-- <h1>{{ i*2 }}</h1> --> # 오류발생함
    <!-- <h1>{#{ i*2 }#}</h1> --> # DTL 주석처리
    {% comment %} <h1>{{ i*2 }}</h1> {% endcomment %} # DTL 전체 주석처리
    ```

  * for 태그

    * 반복을 위한 태그

      ```
      {% for 임수변수 in iterable한 객체 %}
      {% endfor %}
      ```

    * for empty

      ```
      {% for 임시변수 in iterable한 객체 %}
      	값이 하나라도 있으면 여기가 실행
      {% empty %}
      	출력할 값이 없으면 출력.
      {% endfor %}
      ```

  * if 태그

    * 조건을 구분 하기 위한 태그

      ```
      {% if 조건문 %}
      {% elif 조건문 %}
      {% else %}
      {% endif %}
      ```

  * 나머지 기타 유용한 DTL 문서를 참고.(구글검색 키워드 : django, built-in template)

* Form

  * HTML form tag 의미

  * 입력 받은 데이터를 어딘가로 보낼 때 사용.

    ```
    <form action="/test/" method="GET"> # action : 어디로 보낼 것인가 / GET : 데이터조회, # action="//" 슬래쉬 두개 잊지말자
    # POST : 데이터 변경, 비밀번호 등
    
    	input 데이터를 입력 받게 적절히 코딩하면 됨.
    	
    	# 오락실 버튼(버튼 모양만 있음. 의미부여 하기전까진 의미 없음.)
    	<input type="button">
    	
    	# 미사일 버튼(버튼 자체에 의미가 있음.)
    	<input type="submit">
    			or
    	<button></button>
    </form>
    ```

  * action에 들어가는 목표 url 설정 주의 사항

    ```
    action="/catch/"
    => 127.0.0.1:8000/catch/
    
    action="catch/"
    => 현재 주소 : 127.0.0.1:8000/index
    => 127.0.0.1:8000/index/catch
    ```

    

* 기본 흐름을 잘 기억하자!!

