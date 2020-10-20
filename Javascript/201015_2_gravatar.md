#### gravatar



hash_check.py

https://ko.gravatar.com/site/implement/images/python/

```
import hashlib

email = 'eunji0yi@gmail.com'

hash_email = hashlib.md5(email.lower()).hexdigest()

print(hash_email)
```



$ python hash_check.py
Traceback (most recent call last):
  File "hash_check.py", line 5, in <module>
    hash_email = hashlib.md5(email.lower()).hexdigest()
TypeError: Unicode-objects must be encoded before hashing
(venv)



이메일을 인코딩하자.

```py
import hashlib

email = 'eunji0yi@gmail.com'

hash_email = hashlib.md5(email.encode('utf-8').lower()).hexdigest()

print(hash_email)
```

그러면 해시값 생김

$ python hash_check.py
23f10968a1c1762dce225e0bed26fe7e
(venv) 



*https://www.gravatar.com/avatar/23f10968a1c1762dce225e0bed26fe7e* 이 경로로 브라우저에서 들어가보면

그라바타 전원 마크 모양 보인다.

이제 이걸 바꾸자.



로그인 후 개발자리소스로 가기

https://ko.gravatar.com/site/implement/  Gravatar 이미지 요청 클릭

https://ko.gravatar.com/site/implement/images/



https://www.gravatar.com/avatar/23f10968a1c1762dce225e0bed26fe7e?d=wavatar

뒤에 ?d=키워드 입력

https://www.gravatar.com/avatar/23f10968a1c1762dce225e0bed26fe7e?d=monsterid 등등

이제 이 이미지를 프로필 페이지에다가 보여주자.





구글링 django custom filter <= 매우 유용

https://docs.djangoproject.com/en/3.1/howto/custom-template-tags/

accounts 앱 폴더밑에 templatetags 폴더 만들고(=앱이랑 같은 위치이면 된다.) 그 안에 빈 파일 `__init__.py` 만든다. 그리고 templatetags 폴더 안에 gravatar.py라고 파일만들기(이건 이름 달라도 됨)

templatetags를 만들면 자동으로 재시작해주지 않기 때문에 서버 껐다가 키는거 수동으로 해야한다. (`python manage.py runserver`)



gravatar.py

```python
from django import template
import hashlib

register = template.Library()

@register.filter
def get_gravatar(value): #value안에 email이 들어간다. {{ person.email|get_gravatar}} 에 의해서.
    url = 'https://www.gravatar.com/avatar/'
    hash_value = hashlib.md5(value.encode('utf-8').lower()).hexdigest() #value를 인코딩해서 소문자로 바꾼다음에 해시로 만들어서 리턴
    return f'{url}{hash_value}?d=wavatar'
    # 리턴하는 부분이 {{ person.email|get_gravatar}}로 반환이 된다. 
```

