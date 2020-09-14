# Loading CRUD...



* 가상 환경 설정

  * `python -m venv venv`
  * vscode 에서 interpreter 설정.
  * 가상환경 실행. 
    1. vscode 새로운 터미널창 오픈 (vscode 자동 실행)
       * 실행 안되면 쓰레기통 눌러 종료 후 다시 실행.
    2. `source venv/Scripts/activate` 실행 (수동 실행 방법)

  * pip list 로 깨끗한지 확인.
  * `pip install django`

----

* 준비 단계
  * 프로젝트 생성
    * `django-admin startproject 프로젝트명`
    * `manage.py` 가 있는 폴더로 이동
    * `python manage.py runserver` 로 로켓 확인!
  * 앱 생성
    * `python manage.py startapp 앱이름`
    * 바로 `settings.py` 열어서 앱을 등록.
  * url 분리 작업
  * 공통적으로 사용할 `base.html` 생성
    * TEMPLATE 에 있는 DIRS 에 폴더의 위치를 등록

----

* 간단한 페이지 작성 방법!!!

  * `urls.py -> views.py -> templates/html파일 ` 루틴으로 모든 장고 기능 작성을 할 것임.
  * `html`에서 값을 보여주고 싶을 때는 `views.py`에서 `render의 세번째 인자`로 `dictrionary 형태`를 가지는 값을 넘겨 주면 됨. (변수 명을 context으로 보통 전달함.)
    * html 에서 보여줄 때는 `{{ 보내는 키명 }}` 으로 나타낼 수 있다.

  * Variable Route 
    * 주소 중 일부를 변수로 사용. 즉 패턴에 있는 주소 값을 변수에 저장할 수 있다.
    * 즉, 내가 원하는 값을 주소로 전달 할 수 있다.
    * `urls.py` 에서는 주소 패턴 에 `<타입:변수명>` 정의
    * `views.py`에서는 함수 매개변수 명을 정해주는데 반드시 `urls.py`에서 설정한 변수명 으로 해야함.

----

* Model 사용
  * class 를 작성을 한다.
  * `python manage.py makemigrations` : DB 설계 도면을 생성.
  * `python manage.py migrate` : DB 생성하기



* Admin 페이지 사용해 보기
  * `admin.py` 에 내가 만든 모델을 등록
  * 관리자 계정을 생성. `python manage.py createsuperuser`



-----

* CRUD (Form 은 없이)

  * Read 

    * 전체 목록 읽어 오기 (index)
      * QuerySet 형태로 데이터를 받아옴.
    * 하나의 목록만 읽어 오기 (detail) 
      * 해당 데이터의 pk 값이 필요. => variable route 를 이용해서 전달.
      * Object 형태로 데이터를 받아옴.

  * Create

    * request method 로 할일을 나누는데
      1. 링크를 누르거나, 주소창에 주소를 입력했을 때 : GET
         * 입력할 수 있는 페이지를 보여주세요.
      2. 제출 버튼을 눌렀을 때 : POST 
         * 데이터를 DB에 저장해주세요.

    * form action 에 값이 없으면 현재 주소로 데이터를 전달함.

    * 데이터는 `request` 에 들어 있다.

      * GET 방식으로 데이터를 전달 받으면 `request.GET`
      * POST 방식으로 데이터를 전달 받으면 `request.POST`
      * 해당 값은 QueryDict  형태로 전달이 된다. 즉, Dictionary 라고 생각하면 편함.
      * Dictionary 에서 키 값을 이용해 value 를 찾아오는 `get` 함수를 이용해서 값을 가져옴.

    * DB 에 데이터를 저장하는 방법 3가지

      1. 첫번째 방법

         ```python
         board = Board() # 인스턴스 생성
         board.title = request.POST.get('title') # 값을 넣고
         board.save() # 세이브 필
         ```

      2. 두번째 방법

         ```python
         board = Board(title=request.POST.get('title')) # 인스턴스 생성과 동시에 값 할당
         board.save() # 세이브 필
         ```

      3. 세번째 방법

         ```python
         board = Board.objects.create(title=request.POST.get('title')) # create 함수 사용하면 바로 DB에 저장됨.
         ```

         

  * Update

    * Create 와 동작은 동일하다

    * 차이점은 DB 에서 값을 가져와서 보여주느냐 아니냐의 차이

    * 수정된 데이터를 저장하는 방법

      ```python
      board = Board.objects.get(pk=pk) # 우선 수정할 녀석을 가져온다.
      board.title = request.POST.get('title') # 기존의 값을 받아온 값으로 덮어쓴다.
      board.save() # 세이브 필
      ```

  * Delete
    * DB 를 삭제하는 동작이기 때문에 POST 방식을 이용.

