# Django

Django 3.1

`pip list`

 \* 장고 설치하기

```txt
pip install django

* 특정 버전으로 받고 싶다.
pip install django==3.0.8
```



\* 장고 시작하기

```txt
django-admin startproject first_webex
```

* `first_webex`라는 이름으로 폴더가 생성

  * 여기 안에는 first_webex 폴더와 manage.py 생성 되어짐.

    * first_webex: 프로젝트 설정 파일들이 들어 있음.

    * manage.py : 장고 명령어를 실행하기 위한 파일.

      `python manage.py 명령어`

    * 가장 바깥에 있는 프로젝트 폴더명은 수정 가능 하나 .setting파일이 들어 있는 폴더명은 건드리지 말자. 보통 프로젝트 폴더명에 _pjt를 붙여서 사용한다.

\* 장고 실행하기

```txt
python manage.py runserver
```



* 장고 프로젝트는 Application의 집합체로 동작

  * 실제로 어떤 역할을 해주는 친구가 바로 app
  * 하나의 프로젝트는 여러 개의 app을 가질 수 있음.
    * app: 하나의 역할 및 기능 단위로 쪼개진 형태
      * 회원 관리 / 글 작성, 수정, 삭제 / 데이터를 수집 분석
      * 어플리케이션을 어떻게 나눠야 하는지에 대한 절대적 기준은 없음.
      * 작은 프로젝트는 app을 따로 나누지 않아도 된다.

* 어플리케이션 생성

  ```
  python manage.py startapp 앱이름(복수형)
  ```

  * 해당 앱 이름으로 폴더가 생성됨

  * 바로 할 일이 있다.

    * setting.py에 installed_apps에 생성한 앱을 등록해줘야함

    * LANGUAGE_CODE = 'ko-kr' (한글로 설정)

    * TIME_ZONE = 'Asia/Seoul' (시간 아시아 서울로 설정), 첫글자는 대문자로 작성

      

* MVC패턴(Django에서는 MTV)

  * Model - View - Controller
  * Model- Template - View

* 3대장 : 우리가 가장 밀접하게 수정하여야 하는 파일 명

  1. urls.py

  2. views.py

  3. templates(html들)

     

* urls.py에서 해야할 일

  * path('url패턴/', 실행되어야 하는 views에 있는 함수, 해당 path의 별명)
  * 많이 놓치는 부분 : url패턴 뒤에 `/`

  

* views.py에서 해야할 일

  * 함수를 정의 해야한다.
  * 첫번째 인자로 request를 받는다.
  * 두번째 인자로 템플릿을 넣는다.



## Django Template Language(DTL)

* django template system에서 사용하는 built-in template system이다.
* 조건, 반복, 치환, 필터, 변수 등의 기능을 제공
* 프로그래밍적 로직이 아니라 프레젠테이션을 표현하기 위한 것
* 파이썬처럼 if, for를 사용할 수 있지만 이것은 단순히 python code로 실행되는 것이 아닙니다.



### syntax

* variable : `{{ var }}`
* filter : `{{ var|filter }}`
* tags : `{% tag %}`



### 템플릿 시스템 설계 철학

* django는 템플릿 시스템이 **`표현`**을 제어하는 도구이자 표현에 관련된 로직일뿐이라고 생각한다.
* 템플릿 시스템에서는 이러한 기본 목표를 넘어서는 기능을 지원해서는 안된다.



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

