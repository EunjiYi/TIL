###  Account 간단 요약

* 앱을 새롭게 생성
* **Article**이 이미 존재하지만 **게시글**을 관리하는 역할을 하는 앱
* 회원관리 역할



### 회원가입 기능 추가

* 회원가입 == DB 유저 정보를 새롭게 추가하는 행위(Create)

* **UserCreationForm**: django에서 기본적으로 제공해주는 폼
  * 유저 정보를 DB에 저장을 해야함 -> ModelFom
  * `from django.contrib.auth.forms import UserCreationForm`
  * 나머지 로직은 이전 CRUD의 create와 동일
  
  
  
* 회원 가입과 동시에 로그인이 되야 한다.

  * 회원가입을 한다는 것은 우리 사이트의 회원임이 인증된 것이다. 

  * 즉, 회원가입해서 생성된 정보(save)를 넘겨주면 됨!

  * views.py에서 def signup에서

    ```python
            form = UserCreationForm(request.POST)
            if form.is_valid():
                user = form.save()
                # 이때부터 유저는 인증된 유저다.
                auth_login(request, user)
    ```

    

### 로그인

* 쿠기

  * 브라우저에 저장
  * 키-벨류 쌍으로 저장되는 작은 데이터파일
  * 만료일자
  * 쿠키 종류
    * 세션 쿠키
      * 브라우저 쿠키라면 보통 세션 쿠키라고 생각하면 됨.
      * 사용자가 사이트를 탐색할 때, 설정과 선호 사항을 저장하는 임시 쿠키
      * 브라우저를 닫으면 삭제됨
    * 지속 쿠키
      * 사용자가 주기적으로 방문하는 사이트에 대한 설정 정보나 로그인 이름을 유지하기 위해 주로 사용
      * 브라우저를 닫거나 컴퓨터를 재시작해도 남아 있음. 
      * 만료일자가 지나거나, 따로 삭제를 해주면 삭제됨

* 세션

  * 서버 DB 혹은 메모리
  * 정말 중요한 정보를 저장
  * 사용자가 많아지면 서버가 느려질 수 있음.

  

* 로그인 == 세션을 새롭게 생성(Create)

* AuthenticationForm: Django에서 기본적으로 제공해주는 Form

  * 로그인을 위해서 입력창을 제공
  * 로그인 유효성 검사
  * DB 유저 정보와 비교해서 인증을 해주는 친구
  * DB를 직접 수정하는 폼이 아니라서 form
    * 첫번째 인자로 request 정보를 보내야함.

* **login 함수**: Django에서 기본적으로 제공해주는 함수

  * 세션에 인증 정보를 생성해주는 함수.
  * 로그인 == 세션을 새롭게 생성(Create)해주는 함수





### 로그아웃

* 로그아웃 == 세션을 삭제(Delete)
* logout 함수: Django에서 기본적으로 제공해주는 함수
  * 현재 request에서 session에 관한 data를 삭제.



### 접근제한

* `is_authenticated`
  * User 클래스와 AnonymousUser의 속성값
    * 로그인 안한 상태면 {{ request.user }} 값이 AnonymousUser 라고 나옴.
    * User는 해당 값이 항상 True, AnonymousUser는 항상 False 반환. (djnago github 소스코드 참고)
  * 유저가 인증된 유저인지 아닌지를 확인
* `is_anonymous`: 유저가 인증되지 않은 사용자인지 확인.
* `is_superuser`: 유저가 최고관리자 인지 확인
* `is_staff`: 유저가 관리자 계정에 접근 가능한지 확인.



* login_required 데코레이터
  * 로그인이 된 유저만 해당 함수를 실행가능하게 하는 데코레이터
  * 만약 로그인이 되지 않은 유저라면
    * `accounts/login/` 으로 리다이렉트 해줌.
    * next  라는 쿼리 문자열에 이전에 접근하려했던 주소를 keep해줌.
      * keep 된 주소를 사용하려면 request.GET.get('next')해서 리다이렉트
    * `@login_required(login_url='/accounts/test/')`
    * settings.py 에 LOGIN_URL을 설정을 해주면 해당 주소로 갈 수 있음.
      * LOGIN_URL의 기본값이 `accounts/login`



* login_required와 require_POST를 같이 사용할 수 없는 이유.

  ```python
  @require_POST
  @login_required
  def logout(request):
      # if request.user.is_authenticated:
      auth_logout(request)
      return redirect('accounts:index')
  ```

  * 비로그인 상태로 POST로 logout을 시도했을 때
    1. login_required 에서 로그인 페이지로 리다이렉트(POST 데이터 손실... = 만약 이게 지원페이지 자소서라면...)
       * 리다이렉트는 GET 방식
    2. 로그인을 완료 후 next를 이용해서 다시 logout으로 접근
       * 리다이렉트로 logout을 접근하게 됨
    3. 결국 405 http method 에러가 발생.



### 회원탈퇴

* 회원 탈퇴 == DB에서 유저 정보를 삭제

* 이전에 데이터베이스로 정보를 삭제하는 방법

  ```python
  def delete(request, id):
      data = Article.objects.get(pk=id) # article 정보를 가져와서
      data.delete() # article 정보에서 delete를 실행
      ...    
  ```

  

* 유저 정보는 request.user에 담겨져 있다.
  * request.user.delete()를 하면 유저 정보가 삭제.
  * DB 정보를 삭제하는 것이기 때문에 POST 요청.



### 회원 정보 수정

* 회원 정보를 Update

* `UserChangeForm` : Django에서 기본적으로 제공해주는 폼
  * DB를 수정해야하는 폼
  * ModelForm
  * 문제점
    * 일반유저가 권한 설정을 할 수 잇게 됨
    * 그대로 사용하면 절대 안 됨
* `CustomUserChangeForm` : `UserChangeForm`을 상속받아서 커스텀을 한 폼.
  * 원하는 필드만 수정할 수 있게 해야함.
  * 유저의 정보를 채워서 입력창을 보여줘야 하므로 `instance`설정을 한다.



#### tip

* 디버깅 순서
  * 개발 순서 : 요청 -> url -> view -> template -> 응답
  * 개발 순서의 역순으로 오류를 트래킹
    * 응답(오류메세지) -> template -> views -> url -> 요청(주소줄 확인 or 장고 터미널 로그 확인/ log에 찍힌 요청을 확인)





### 비밀번호 변경

* DB를 수정을 하는데 그런데...텍스트 그대로 저장하면... 초토화
  * Django는 비밀번호를 그대로 저장하지 않고 암호화를 시킨다. 
* PasswordChangeForm
  * Form을 상속받아서 정의한다. 
  * 첫번째 인자로 `request.user`가 반드시!!! 필요하다.
  * data=request.POST 를 넣어서 사용한다. `data=` 이건 안쓰고 걍 `request.POST`만 써도 된다.
* 비밀번호 변경을 성공하게 된다면 로그인이 풀려버린다.
  * 로그인이 되면 유저 정보를 세션에 저장하는데
  * 비밀번호가 변경이 되면 유저 정보가 업데이트가 되어서 세션에 저장된 유저정보와 데이터가 일치하지 않는다. 
  * `update_session_auth_hash`함수를 사용해서 세션의 유저 정보를 업데이터 시켜줘야한다.

