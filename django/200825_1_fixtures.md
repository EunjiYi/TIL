[toc]

# fixtures

> Django가 데이터베이스로 import 할 수 있는 데이터 모음
>
> 앱을 처음 설정할 때 데이터베이스를 미리 채워야 하는 상황이 존재하는데 이러한 초기 데이터를 제공하는 방법 중 하나
>
> [https://docs.djangoproject.com/en/3.1/howto/initial-data/#providing-initial-data-for-models](https://docs.djangoproject.com/en/3.1/howto/initial-data/)



## **fixtures 출력 및 로드**

**dumpdata**

> [https://docs.djangoproject.com/en/3.1/ref/django-admin/#dumpdata](https://docs.djangoproject.com/en/3.1/ref/django-admin/)

- 특정 앱의 관련된 데이터베이스의 모든 데이터를 출력



**loaddata**

- dumpdata를 통해 만들어진  fixtures 파일을 데이터베이스에 import
- **fixtures 파일은 반드시 app 디렉토리 안에 fixtures 디텍토리에 위치**



**dumpdata usage**

- 명령어

  ```bash
  $ python manage.py dumpdata app_name.ModelName [--options]
  ```

- 예시

  ```bash
  $ python manage.py dumpdata articles.Article --indent 4 > articles.json
  ```

  

**loaddata usage**

- 명령어

  ```bash
  $ python manage.py loaddata fixtures_path
  ```

- 예시

  ```bash
  $ python manage.py loaddata articles/articles.json
  ```

  