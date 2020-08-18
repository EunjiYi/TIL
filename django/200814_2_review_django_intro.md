### Django

--------------



python을 기반으로 하는 웹백엔드프레임워크



#### 장고 설치 방법

`pip install django`

* 버전: pip list (django 3.1 확인)



#### 장고프로젝트 생성

`django-admin startproject 프로젝트명`

* 프로젝트 폴더명으로 설정파일이 저장되는 폴더와 manage.py파일 생성

* 외부 프로젝트 폴더명은 수정 가능하지만, 내부 설정파일 폴더명은 수정이 어렵다. 

* 프로젝트 생성완료하면 항상 manage.py가 있는 위치로 이동, 안하면 No such File

* `python manage.py runserver` 를 실행하여 로켓을 보면 됨. (app이 등록되면 볼 수 없다.)



### 장고 앱 생성

`python manage.py startapp 앱이름(복수명 권장)`

* 앱 생성 후 필히!!! `settings.py` 파일에 등록하기
* language_code, time_zone: 한국에 맞춰서 수정



-----------



### 프로젝트의 흐름

------------

#### MTV 패턴 (MVC 패턴)

* Model  ( = Model): DB관리
* Template  (=View): 보여지는 페이지 관리
* View  (=Controller): 데이터를 처리하고 매니징(관리).



흐름을 잃지 말자! 꼭 기억해야한다.



주소가 입력이 잘못되어서 해당 페이지를 찾을 수 없다는 것 = Page not found



* 3대장

  * urls.py
  * views.py
  * models.py

  

* 코드의 작성 흐름

  * urls.py -> views.py -> template 수정
  * 어디에서 주소를 설정하는지! (urls)
  * 요청이 들어오면 어떤 파일을 거치게 되는지
  * 어디에서 새로운 페이지를 만들면 되는지(templates 폴더)

----------------------------------------



* Template Variable

  * render() 사용할 때 views.py에서 정의한 변수를 template파일(html파일)로 넘겨서 사용하는 것.
  * render()의 세번째 인자로 dictionary 형태로 값을 넘겨준다. 여기서 key에 해당하는 문자열이 template에서 사용가능한 변수명이 된다.
  * dictionary 형태로 직접 전달하는 것 보다는 context라는 변수를 사용해서 넘기기 - 일반적

  -----------------------

* Variable Routing

  * 동적 라우팅: url주소 일부에 변수처럼 값을 전달하는 동적인 주소를 만드는 것.
  * 왜 사용할까?
    * hi/justin, hi/john 등과 같이 다양한 사람들과 인사를 하는 함수를 작성할 때
      * 동적 라우팅을 쓰지 않으면 `urls.py`에 일일이 등록해줘야한다.
      *  인원이 많아지거나 누구한테 인사해야하는지 모를 때 고정적인 방식은 사용하기 어렵다.
    * 동적라우팅을 사용하면 뒤에 사람이름을 변수화 할 수 있다.
      * `hi/<str:name>` 형식으로 나타낼 수 있음
      * views.py에서 함수를 정의할 때 인자로 꼭  varible routing에서 선언한 변수명을 받아야한다.
    * 변수로 사용할 수 있는 type의 종류
      * 구글에서 django url dispatcher로 검색하면 확인 가능.



----------

#### Django Templates Language(DTL)

* django template에서 사용하는 내장  tempates system
* 조건, 반복, 변수치환, 필터 등의 기능을 제공
* 로직을 표현할 때 `{% %}`
* 값을 표현할 때 `{{  }}`
* 주석을 표현할 때  `{#  #}` {% comment %} {% endcomment %}



* For문

  * {% for 임시변수 in <뷰로부터 전달 받은 iterable한 변수명> %} {% endfor %}

  * 파이썬은 인텐트로 구분하지만 html은 그런게 없어서 endfor를 지정해준 곳까지 for문의 영력이다.

    ```
    {% for menu in menus %}
    	<p>{{ forloop.counter }} {{ menu }} </p>
    {% empty %}
    	<P> menus에 아무런 값이 없을 때 여기가 실행됩니다. 메뉴가 없습니다. </p>
    {% endfor %}
    ```

    

* if 문

  ```
  {% if 조건문 %}
  {% elif 조건문1 %}
  {% else %}
  {% endif %}
  ```

  * 조건 연산자 사용가능

    ```
    <= 이상
    >= 이하
    == 같다
    != 같지 않다.
    > 크다
    < 작다
    in 들어있다.
    not in 들어있지않다.
    is
    ```



------------

* Form

  * HTML에서 form tag를 의미한다.

  * 입력받은 데이터를 어딘가로 전송할 때 사용한다.

    ```html
    <form action="" method="">
        <input type="text" name="데이터명">
        <input type="radio" name="데이터명">
        ....
        <input type="button"><!--말그대로 그냥 버튼, 오락실버튼느낌 -->
        <input type="submit"> <!--form과 항상 함께다니는 친구: submit-->    
        
        <!--type=button말고 button태그도 submit의 역할을 한다.-->
        <button>
            보내기
        </button>
    </form>
    <input type="text"> <!--form태그 내부 친구들과 같이 전송이 되지 않는다. form~/form까지만 전송됨-->
    ```

  * action: 데이터를 보내려는 목적지 주소 

  * **/가 있는지 없는지 꼭 확인하자**

    * acition="/catch/": `localhost:8000/catch/`
    * action="catch/": `현재주소/catch/`

  * method: http method(GET, POST)

    * GET: 주소에 query string 형식으로 데이터를 전달하는 방식

      > 누가봐도 상관이 없는 정보를 전달할때, `정보를 조회할 때` 주로 사용한다.
      >
      > `localhost:8000/catch/?데이터명=데이터값&데이터명2=데이터값2&...`

  * request 라는 함수를 선언할 때, 넣어주었던 인자에 그 값이 들어있음.

    > request안에 유저가 요청한 정보(값) 다 들어있다.
    >
    > 그래서 정보를 조회할 때 사용한다.

    