django가 데이터베이스로 import 할 수 있는 데이터 모음을 `fixtures`라고 한다. app을 처음 설정할 때 데이터베이스에 초기 데이터를 제공하는 방법 중 하나다. 



# Loading CRUD



* 가장 환경 설정

  * `pythom -m venv venv`
  * vscode에서 interpreter 설정
  * 가상환경 실행
    1. vscode 새로운 창 오픈
    2. `source venv/Scripts/actuvate`실행
  * pip list 로 깨끗한 상태인 것 확인
  * `pip install django `

  

* 준비단계

  * 프로젝트 생성
    * `django-admin startproject remember_crud`
    * `cd remember_crud` (manage.py 가 있는 폴더로 이동하는 것)
    * `python manage.py runserver` -> 로켓확인

  * 앱 생성
    * `python manage.py stratapp 앱이름`
    * 바로 `settings.py`에 INSTALLED_APPS 등록
    * 같은 곳 하단에 LANGUAGE_CODE = 'ko-kr' 랑
    * TIME_ZONE = 'Asia/Seoul' 바꾸기 
  * url 분리 작업
  * 공통적으로 사용할 base.html 설정
    * TEMPLATE 에 있는 DIRS에 폴더의 위치를 등록

-------------------------



### 간단한 페이지 작성 방법

* url.py -> view.py -> template/html 파일 루틴으로 모든 장고 기능을 작성하자!
* html에서 값을 보여주고 싶을 때는 views.py 에서 render의 세번째 인자로 dictionary 형태를 가지는 값을 넘겨주면 된다. (변수명을 context로 전달함)
  * html에서 보여줄 때는 `{{보내는 키명}}`으로 나타낼 수 있다.
* Variable Route
  * 주소 중 일부를 변수로 사용, 즉 패턴에 있는 주소 값을 변수에 저장할 수 있다.
  * 즉 내가 원하는 값을 주소로 전달할 수 있다.
  * urls.py에서는 주소 패턴에 `<타입:변수명>` 정의
    * str, int
  * `views.py`에서는 함수 매개변수명을 정해주는데 반드시 `urls.py`에서 설정한 변수명으로 해야함.



Model 사용

* class를 작성한다.
* `python manage.py makemigrations`  : DB 설계도면을 생성
* `python manage.py migrate`: DB 생성하기



Admin 페이지 사용해보기

* admin.py에 내가 만든 모델을 등록
* 관리자 계정을 생성. python manage.py createsuperuser



--------------



### CRUD

* Read
  * 전체 목록 읽어오기(index)
    * QuerySet 형태로 데이터를 받아옴
  * 하나의 목록만 읽어오기(detail)
    * 해당 데이터의 pk값이 필요 -> variable route 를 이용해서 전달
    * Object 형태로 데이터를 받아옴 - QuerySet 중 한 가지(pk)만 가져오니까.



* Create
  * request method 로 할 일을 나누는데
    * 링크를 누르거나, 주소창에 주소를 입력했을 때 : GET
      * 입력할 수 있는 페이지를 보여주세요.
    * 제출 버튼을 눌렀을 때: POST
      * 데이터를 DB에 저장해주세요.