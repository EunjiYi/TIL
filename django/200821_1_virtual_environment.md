QeurySet API : all, filer, get, order_by 등등



### 가상환경: 

독립적인 환경에서 진행 . 라이브러리와 모듈간의 의존성 때문에, 또는 서로의 가상환경의 동기화 때문에 한다.fixtures: 장고가 데이터베이스에 import할 수 있는 데이터들의 모음(data collections)

> 파이썬 인터프리터, 라이브러리 및 스크립트가 글로벌환경(시스템파이썬=운영체제의 일부로 설치되어 있는 것)에 설치된 모든 라이브러리와 격리되어있는 파이썬 환경

각 가상환경은 `고유한` 파이썬 환경을 가지고, `독립적`으로 설치된 패키지 집합이다.

생각보다 조금 무겁다. (용량도 크다)

종류: venv(python 3.3부터 venv가 기본 모듈로 내장) virtualenv conda pyenv(maxOS) 등



#### 가상환경을 사용하는 이유

pip로 설치한 패키지들은 lib/site-packages안에 저장되어있는데 이는 모든 파이썬 스크립트에서 사용할 수 있다.

1> 여러 프로젝트를 진행하게 되면 프로젝트마다 다른 버전의 라이브러리가 필요할 수도 있다. 그런데 `파이썬에서는 한 라이브러리에 대해 하나의 버전만 설치가 가능`

2> `각 라이브러리나 모듈은 서로에 대한 의존성(dependency)가 다르기 때문에` 알 수 없는 충돌이 발생하는 등의 다른 여러 문제가 생김.



#### 가상환경 만들기

$ python -m venv venv(=가상환경이름도  venv 로 하는 편) 

( 파이썬 3.7.2 이상이어야 이 명령어 먹음)



#### 가상환경 키기

$ source venv/Scripts/activate (윈도우기준 가상환경이름\Scripts\activate)

(venv)

macOS는 가상환경이름/bin/activate



#### 가상환경 확인 - 완전히 새로운 깨끗한 환경이다.

$ pip list

Package  Version

`---------- -------`

pip    19.2.3

setuptools 41.2.0

WARNING: You are using pip version 19.2.3, however version 20.2.2 is available.

You should consider upgrading via the 'python -m pip install --upgrade pip' command.

(venv)





#### 가상환경끄기/비활성화

$ deactivate



_주의_

venv avtivate에 지금 가상환경 경로가 저장되어있다. 

`그러니까 한 번 가상환경 만들면 그 폴더 막 이동시키면 안됨.!`



파이썬 위치찾기

$ which python

/c/Users/EUNJI YI/AppData/Local/Programs/Python/Python37/python



글로벌환경에서 pip list 찍으면 나오는 라이브러리(패키지)들은 여기에 물리적으로 설치가 된다. 

C:\Users\EUNJI YI\AppData\Local\Programs\Python\Python37\Lib\site-packages



그렇다면 가상환경 pip list찍으면 나오는 것들은?

C:\Users\EUNJI YI\Desktop\사피\django 200814\0821\venv\Lib\site-packages

즉 venv폴더 자체가 하나의 환경인것!



#### VS CODE에서 가상환경 쓰기

 ctrl+shift+p

select interpretor 클릭

Python 3.7.7 64-bit (venv:venv) 가상환경에 있는 파이썬 쓰자.

그리고 메뉴바 Teminal -> new teminal 하면 이렇게 알아서 소스해줌

$ source "c:/Users/EUNJI YI/Desktop/사피/django 200814/0821/venv/Scripts/activate"

(venv)

그리고 vs code 좌측 하단에, Python 3.7.7 64-bit ('venv':venv) 파란색 줄에 떠있는지 확인하자.



#### 프로젝트 순서

가상환경을 git bash로 만들기 -> vscode 실행 -> interpreter 셋팅 -> teminal 키기



이 상태에서 장고설치하자.

여기서 바로 아래 명령어 하면, 글로벌 환경의 장고를 설치한다. 

$ django-admin startproject crud .

(venv) 



pip로 가상환경 전용 장고 설치해야한다.

(vnev)인 상태에서,

pip install django 하고

pip list로 설치확인



django-admin startproject crud .

현재 환경에 있는 장고를 가지고 프로젝트 만든 것.

`가상환경에서는 반드시 .찍어서 그 가상환경폴더(vnev)안에 프로젝트 생겨야한다.`



pip install requests

pip install ipython django-extensions



#### git init

$ git init

Initialized empty Git repository in C:/Users/EUNJI YI/Desktop/사

피/django 200814/0821/.git/

(venv)

EUNJI YI@LAPTOP-ON7NAJRQ MINGW64 ~/Desktop/사피/django 200814/0821 (master)



#### git status

EUNJI YI@LAPTOP-ON7NAJRQ MINGW64 ~/Desktop/사피/django 200814/0821 (master)

$ git status

On branch master

No commits yet

Untracked files:

 (use "git add <file>..." to include in what will be committed)

​    crud/

​    db.sqlite3

​    manage.py

​    venv/

nothing added to commit but untracked files present (use "git add" to track)

(venv)



#### gitignore

gitignore.io에 어떤 파일들이 들어가는지 알아보자.



OS IDE 언어 Framework 가상환경 큰거에서 작은 순으로 적어주자.

가장 기본: gitignore.io에서 Windows macOS VisualStudioCode PyCharm Python Django venv 치고

이렇게 해서 생긴 파일 복붙

그리고 git status 다시 하면,

```
    .gitignore
     crud/
     manage.py
```

$ git remote add origin https://github.com/EunjiYi/venv-test.git

(venv)

EUNJI YI@LAPTOP-ON7NAJRQ MINGW64 ~/Desktop/사피/django 200814/0821 (master)

$ git remote -v

origin https://github.com/EunjiYi/venv-test.git (fetch)

origin https://github.com/EunjiYi/venv-test.git (push)(venv)



### 패키지 관리

pip freeze

> 현재 환경에 설치된 패키지를  requirements format으로 출력
>
> 각 패키지들은 대소문자를 구분하지 않고 알파벳 순서로 나열

패키지 요구사항 파일 생성

> pip freeze > requirements.txt

패키지 요구사항 설치

> pip install -r requrements.txt



#### pip list와 pip freeze의 차이는?

$ pip freeze > requirements.txt

freeze는 출력값을 리다이렉트가 가능하다.

freeze를 아웃풋포맷이라 한다.

pip list와 pip freeze의 `역할이 다르다`고 보면 된다.

> 터미널에서 설치된 패키지의 목록을 보실 때는 pip list
>
> 그리고 다른 사람과 공유하기 위해 파일로 해당 목록을 저장할 때는 pip freeze를 사용해주시면 됩니다.



환경에서 패키지가 업데이트 되면 매번 freeze해서 업데이트 해줘야함. 항상.

requirements.txt는 꼭 git push해서 같이 올린다. 



----------------

그리고 다른 사람이 clone받고 나서 clone받은 그 폴더경로에서,

python -m venv venv

source venv/Scripts/activate

pip install -r requriments.txt

하면 끝

pip list하면 똑같이 설치되는 것을 확인할 수 있다.

즉 가상환경까지 셋팅 완료



엇 그런데!

db를 안 올리기 때문에 초기 데이터 = initail data 만들어야한다.



즉 co-work을 위해서는

1. 가상환경도 맞춰줘야하고

2. db도 맞춰줘야한다.- db를 안올리는 거지, migrations은 올라감. 설계도니까.

-----



### fixtures

> django가 데이터베이스로 import 할 수 있는 데이터의 모음( = data collections)
>
> 앱을 처음 설정할 때 데이터베이스를 미리 채워야 하는 상황이 존재하는데 이러한 초기데이터를 제공하는 방법 중 하나다.



fixture 출력 및 로드 `dump -> ㅁ -> load`

* dumpdata

  > 특정 앱의 관련된 데이터베이스의 모든 데이터를 출력

* loaddata

  > dumpdata를 통해 만들어진  fixtures 파일을 데이터베이스에 import
  >
  > fixtures 파일은 반드시 app 디렉토리 안에 fixtures 디렉토리에 위치 `app/fixtures`

* dumpdata usage

  * 명령어: `python manage.py dumpdata app_name.ModelName [--options]`
  * 사용예시: `python manage.py dumpdata articles.Article --indent 4 > articles.json`

* loaddata usage
  * 명렁어: `python manage.py loaddata fixtures_path`
  * 사용예시: `python manage.py loaddata articles/articles.json`



`python mange.py dumpdata articles(앱이름).Aricle(models.py에 있는 클래스이름) --indent 4> articles(다른 이름이어도 됨).json `

--indent 4 안하면 한 줄로 나와서 사람이 보기 힘듬. 4칸이 json 기본이니까.

최상단에 articles.json 생김 = django database에 import 될 수 있는 파일

> `articles.json `
>
> 이건 직접 만드는 용도가 아니야.
>
> 애초에 fixtures자체가 그런 목적이 아니야. dump떠서 load 하는게 목표야.
>
> 초기에 dump하기전에 초기데이터가 많이 필요하면, csv형태로 만들어서 load하고, dump하세요





admin 계정 정보도 같이 넘겨주자.

python manage.py dumpdata auth.User --indent 4 > users.json

비밀번호는 따로 알려줘야함.

app폴더/fixtures안에 넣어야한다. = 직접 폴더 만들어서 넣어주자.



articles/fixtures/

그런데 app이 여러개라면

namespace 고려해서

articles/fixtures/articles폴더 만들어서 이 안에, articles.json과 user.json 넣기

그러면 경로적을 때 'articles/articles.json'여기만 적으면 됨



------

---------

자 다른 사람이 clone을 받았다

그러면 이제 venv와 db가 없다. venv는 위의 방벙으로 설치하면 그만이고,

migrations은 이미 들어있으니까 makemigrations는 안해도되고

python manage.py migrates만 하고 

python mangae.py runsever해서 admin 들어가자

그런데 로그인이 안된다. 왜냐면 db가 비어있으니까.



dumpdata와 loaddata모두 여러개 파일 동시에 된다. 그냥 띄어쓰기로 쭉 적어주면 됨. 

python manage.py loaddata articles/articles.json articles/users.json 

이러면 데이터가 들어간 것!

articles_articles랑

auth_uear에 데이터 들어간다. (sqlite로 확인하면 됨.)

이제 admin로그인 가능해짐.