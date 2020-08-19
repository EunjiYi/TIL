장고는 모델을 통해서 데이터에 접속하고 관리한다.

모델을 통해서! 한다.

장고가 직접 ㄴㄴ

장고에 있는 모델이 데이터베이스를 관리한다.

데이터베이스 = 테이블의 집합



sql에서 값이 없음을 표현하는 것 = null파이썬은 none



우리가 파이썬만 쓰면orm이 알아서 db 조작함



테이블 이름 규칙 - 내부적으로 동작한다.

소문자로



스키마: 데이터베이스 테이블에 대한 스트럭쳐(구조)를 스키마라고 한다.



장고 프로젝트 안에서 실행되는 쉘이 필요해

django shell

외장 라이브러리로 shell_plus를 사용하자.



pk랑 .id랑 완전동일

그런데 pk가 권장사항

pk = id_exact의 shoortcut



수퍼유저만들기
python manage.py createsuperuser 



### CharField(max_length=None) 기본값

* 길이에 제한이 있는 문자열을 넣을 때 사용

*  max_length 가 필수인자

* str임. 이름이 char 인 것

* 필드의 최대 길이, 데이터베이스의 django의 유효성validation검사(데이타가 저장되기 전에 데이터가 옳바른 데이터인지 검사(ex 여기서는 이 데이터가 10자가 넘는지 안넘는지)  

  * > 내부적으로 MaxLengthValidator라는 장고의 내부 유효성검사가 실행된대요.

* `input type text`



### TextField()

* 필수인자는 따로 없다. 

* 글자의 수가 많을 때 사용
* 길이의 제한이 따로 없다.
* `<textarea>`



### DateTimeField()

* 최초 생성(first created) 일자: `auto_now_add=True`
  * django ORM이 최초 데이터 입력 시에만 현재 날짜와 시간으로 갱신
  * 테이블에 어떤 데이터를 최초로 넣을 때
* 최종 수정 일자 `auto_now`
  * django ORM이 save를 할 때마다 현재 날짜와 시간으로 갱신
* DateField를 상속받아서 사용함.-시간만 추가된 것 (안중요 그냥 참고)
*  Python에서 datetime.datetime instance와 동일하게 생성이 된다.



 python manage.py makemigrations  -> models.py가 수정되면 매번 해줘야함

```
migrations 폴더: 그때 그때의 모델의 변경사항을 저장해두는 것 git commit 느낌

```



$ python manage.py sqlmigrate articles 0001 -> 확인

```sql
$ python manage.py sqlmigrate articles 0001
BEGIN;
--
-- Create model Article
--
CREATE TABLE "articles_article" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "title" varchar(10) NOT NULL, 
"content" text NOT NULL, "created_at" datetime NOT NULL);
COMMIT;
```



 python manage.py migrate 을 하면 migrations폴더의 최종 마지막 파일이 반영된다.





vs code extension `sqlite`



### Migrations

#### makemigrations

* 모델을 변경한 것에 기반한 새로운 마이그레이션(설계도)를 만들 때 사용
* 모델을 활성화 하기 전에 DB설게도를 작성
* 생성된 마이그레이션 파일은 데이터베이스 스키마를 위한 버전관리 시스템이라 생각하면 된다.



#### migrate

* 작성된 마이그레이션 파일들을 기반으로 실제 DB에 반영하는 작업
* db.sqlite3라는 데이터베이스 파일에 테이블을 생성
* 모델에서의 변경 사항들과 DB 의 스키마가 동기화를 이룸.



#### sqlmigrate

* 해당 마이그레이션 파일이 SQL문으로 어떻게 해석되어서 동작할지 미리 확인하기위한 명령어



#### showmigrations

* 지금 시점에 현재 마이그레이션이 어디까지 됐는지 확인
* 마이그레이션 파일들의 migrate 여부를 확인하기 위한 명령어

```
$  python manage.py showmigrations
...
articles
 [X] 0001_initial
 [X] 0002_article_updated_at
```



### Model의 중요 3단계

1. models.py : 변경사항 발생(작성, 수정, 삭제 등등)
2. makemigrations: 마이그레이션(설계도) 만들기
3. migrate: DB에 적용





### Model

* DB에 데이터를 저장하고 가져오는 것

* SQL이 있어야 데이터베이스 조작이 가능한데, ORM(쉽게 말해서 통역기)가 쿼리(`select * from table;`)를 python에서 object로 사용할 수 있게 해줌.

* `model.py` 에 모델 클래스를 정의해서 사용할 수 있다.

  * class 테이블명(models.Model):

    ​	title = models.CharField(max_length=100)

    * 자주 사용하는 필드명

      > CharField / DataTimeField / TextField / IntegerField / BooleanField / ...
      >
      > django 공식문서: Model field라고 구글링하면 찾아볼 수 있다.



* 클래스를 다 정의를 하면 반드시 해야만 하는 일

  * makemigrations

    * python manage.py makemigrations app이름

      > APP이름을 뒤에 적으면 해당 앱에 있는 models.py의 내용만 설계도를 만듦.
      >
      > 이름 없으면 프로젝트 전체를 다 마이그레이션

    * DB에 적용하기 위한 설계도를 제작한다.

  * migrate

    * python manage.py migrate app이름

      > APP이름을 적으면 해당 에 있는 migration 파일을 DB에 적용시킴

    * 만들어진 설계도를 가지고 DB에 테이블을 생성

  * showmigrations

  * sqlmigrate



###  DB 사용

* DB API

  ```
  모델클래스이름.objects.QuerySetAPI
  
  Artible.objects.all()
  ** objects: s를 잊지 말자. s를 챙깁시다.
  ```

* 자세한 사용방법은 model crud를 참고하면 된다.

  

---------------------





`python -i`

`ipython`



$ pip install ipython django-extensions

> django 확장프로그램(extensions)
>
> ```
> INSTALLED_APPS = [
>     'articles',
>     'django_extensions',
> ```
>
> 

$ pip list로 설치확인하기



`python manage.py shell` //장고 안에서 설정된거라 장고모델을 import 가능

끄기 exit()

`python manage.py shell_plus`

> 장고 자체꺼를 이미 import 다 해 놓음  - 신경안쓰고 편리하게 사용가능해서  shell_plus를 쓰는 것.



