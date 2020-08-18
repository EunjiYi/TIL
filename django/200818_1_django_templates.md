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