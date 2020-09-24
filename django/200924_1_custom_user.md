## Custom User

* 준비( 클래스 정의 / `settings.py`에 등록)

```python
# accounts/models.py

class User(AbstarctUser):
    pass
```

* ~~AbstractBaseUser~~
* AbstractUser

```python
# settings.py
AUTH_USER_MODEL = 'accounts.User'
```

* 기존 DB 삭제
* makemigrations
* migrate

-----------

# Custom User를 했을 시 수정되어야하는 Form

* user를 모델로 하는 모델폼들을 수정해야 함
* 제공되는 user관련 모델폼은 auth.User(Django에서 제공해주는 User 클래스)를 model 정보로 가지고 있기 때문.
* UserCreationForm / UserChangeForm

```python
#accounts/forms.py

class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm.Meta):
        #model = 'accounts.User'
        model = get_user_model()
        fields = UserCreationForm.Meta.fields + ('email')
```

* get_user_model()

  * return 유저 클래스
  * models.py를 제외한 모든곳

  

* settings.AUTH_USER_MODEL

  * return 유저 클래스 문자열(str)
  * models.py 에서 사용



-------

User - Article (1:N)

User - Comment (1:N)

```python
#articles/models.py

class Article / class Comment
	...
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=)
```



----------

sql 공부

https://www.sqlitetutorial.net/