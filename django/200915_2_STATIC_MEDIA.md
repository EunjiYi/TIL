# Static

* **static file** : img, js, css
* 웹에서 사용하기 위해 미리 준비한 파일
* 변경되는 내용 없이 웹 서비스 시 제공해주는 파일



**static 파일을 서브 할려면**

1. 각 앱의 static 이라는 폴더 아래에 정적 파일들이 위치해 있어야 한다.

   (마치 templates 폴더아래에 html 파일이 있는것 처럼.)

   * 앱 폴더 외부에 있는 static 파일이라면 STATICFILES_DIRS 를 settings.py 에 등록.

     (마치 TEMPLATES 설정에서 DIRS 에 base.html 폴더 경로를 등록하는것 처럼.)

2. static tag 를 이용해서 불러와야함.

   ```
   {% load static %} 상단에 있어야함. extends tag 보다는 밑에 있어야 함.
   <img src="{% static "스태틱 파일 경로" %}" alt="My image">
   ```

   * load static , {% static '경로' %} 같은 코드내에 있어야함.

### STATIC_URL

* 웹 페이지에서 사용하는 static file의 최상위 URL 경로
* 실제 파일이나 디렉토리 경로는 아님.
* url 로만 존재하는 단위.
* 반드시 `/` 로 끝나야 함.
* STATIC_ROOT 설정에 있는 static file 을 참조 할 때 사용하는 URL 



### STATIC_ROOT (배포 전용)

* 배포할 때 사용하는 static file 경로
* `python manage.py collectstatic`
  * 프로젝트 배포 시 흩어져 있는 static file을 모아서 특정 디렉토리로 옮기는 작업.
  * 앱 폴더 내부의 static 폴더와 STATICFILES_DIRS 에 등록된 디렉토리의 static file 을 모아줌.
* DEBUG = True 인 경우에는 파일경로로 인식되지 않음. 
  * collectstatic 위치로 이용함.
* DEBUG = False 인 경우 STATIC_ROOT 경로는 인식이 되고 STATICFILES_DIRS 경로는 더이상 인식되지 않는다.



### STATICFILES_DIRS (only 개발용)

* 외부에 있는 static file 을 찾아 오기 위해 등록. 

* like ... base.html 경로 등록하는것과 유사함.

  ```
  STATICFILES_DIRS = [
      BASE_DIR / '스태틱 파일이 있는 폴더명',
  ]
  ```

-----



# MEDIA 

* 사용자가 업로드한 사진을 서비스 하는 방법

* 파일 자체는 static 이지만**, 언제, 어떤 파일을 제공하는지 예측 할 수 없는 파일.**

* ImageField 모델에 추가
  * ImageField 는 장고에서 제공하는 필드
  * 단 사용을 하기 위해서는 `pip install Pillow` 를 해야함.
    * 해당 패키지가 없으면 제대로 동작하지 않음.

* html 에서 form 의 enctype 속성을 설정.
  * application/x-www-form-urlencoded (문자용) : 기본값, 모든 문자 인코딩
  * **multipart/form-data**: 파일 / 이미지 업로드시 반드시 사용해야함.
  * text/plain(문자용) : 인코딩을 하지 않은 문자 그대로 전송. 특수 문자가 인코딩이 되지 않아 알아볼수 없을 수 있다.

* views.py 에서 request.FILES 로 전달 받은 데이터를 form 인스턴스 생성할 때 같이 매개변수로 넣어서 DB 에 저장할 수 있게 해준다.



### MEDIA_ROOT

* 업로드가 끝난 파일을 위치할 최상위 경로를 지정하는 설정.
* 모든 업로드 파일이 해당 파일 아래에 저장이 되어진다.



### MEDIA_URL

* STATIC_URL 과 역할이 비슷하다. 업로드 된 파일의 주소를 만들어 주는 역할
* 일반적으로 `/media/` 로 값을 설정을 함.



### 이미지 표시하기 위한 경로 설정

* [장고 공식 문서](https://docs.djangoproject.com/en/3.1/howto/static-files/#serving-files-uploaded-by-a-user-during-development)

* 실제로 저장된 파일을 사용자에게 표시하기 위해서는 urls.py 수정이 필요하다.

* `urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)`를 설정해서 업로드된 파일에 접근 할 수 있는 경로를 만들어줘야함.

* 해당 부분을 추가해 주지 않으면 프로젝트 내부에 존재하는 업로드 파일로 접근하는 url이 없음.  그래서 해당 부분이 없으면 업로드 받은 파일을 표시할 수 없게됨.

* 위의 내용을 추가하면 `articles.image.url` 로 저장된 파일 경로를 알 수 있음.

* 즉, `<img src="{{ articles.image.url }}" alt="{{ articles.image}}">` 로 불러 올 수 있게됨.

  * articles.image : 이미지 파일 이름
  * articles.image.url : 이미지 접근 가능한 경로.

  

### 이미지 업로드 경로 설정

* media 파일이 업로드 되면 MEDIA_ROOT 경로에 저장이 된다. 

* 추가적인 업로드 경로 설정을 하고 싶으면

* ImageField 의 속성 값인 `upload_to` 에 경로를 지정을 해주면 추가적인 경로를 지정해 줄 수 있다.

  ```python
  # 날짜 경로를 주고 싶을때
  image = ImageField(upload_to="%Y/%m/%d/")
  ```

* 유저명으로 폴더를 관리하고 싶을 때

  ```python
  def articles_image_path(instance, filename):
      return f'user_{instance.user.pk}/{filename}'
  
  
  class Article(models.Model):
      title = models.CharField(max_length=20)
      content = models.TextField()
      image = models.ImageField(blank=True, upload_to=articles_image_path)
      ...
  ```

  * instance : Article 모델 객체값이 들어옴.
  * filename : 업로드한 파일의 이름

----

### 이미지 사이즈 조절

* 사용 패키지 정보

  * Pillow : ImageField 사용시 필수
    * 이미지 파일 형식 지원	
    * 다양한 이미지 처리
  * pilkit : Pillow 를 쉽게 쓸 수 있도록 도와주는 라이브러리. 다양한 프로세서 지원
    * Thumbnail 
    * ResizeToFill : 지정한 사이즈를 맞추고 넘치는 부분은 잘라냄
    * ResizeToFit : 지정한 사이즈를 맞추고 남는 부분은 빈공간으로 둠
  * django-imagekit : Django helper (실제 이미지 처리할때는 pilkit 사용)
    * ProcessedImageField : 원본을 변경해서 처리된 이미지를 저장.
      * parameter로 들어가 있는 값들은 migration 후에 변경이 되어도 다시 migration을 할 필요 없다.
  * 패키지 설치 순서가 중요함. 위에서 차례대로 설치를 권장

* Thumbnail 생성방법

  ```python
  # models.py
  
  from imagekit.models import ProcessedImageField
  from imagekit.processors import Thumbnail
  class Board(models.Model):
      title = models.CharField(max_length=10)
      content = models.TextField()
      image = ProcessedImageField(
          upload_to='boards/images', # 저장 위치 ( media 폴더 이후의 경로 )
          processors=[Thumbnail(200,300)], # 처리할 작업 목록
          format='JPEG', # 저장 포맷
          options={'quality': 90}, # 옵션(원본 대비)
      )
  ```

  



----

**blank vs null**

* null : DB 와 관련 (처음부터 값이 없음)
  * DB 컬럼이 null 을 가질것인지 결정
* blank : 데이터의 유효성 (있었는데 없음)
  * Input tag 안에 requried 를 넣어주어서 체크

- 위와 같은 정의에 의해서 는 null=True, blank=False 옵션을 하나의 필드에서 사용하는것은 문제가 없다. 
- db에서는 Null은 허용하지만, application에서 input 태그의 required 필드 인것을 의미.

* CharField , TextField  문자열을 기반으로 저장하는 필드들!!!!
  * 문자열 필드를 nullable 하게 하고 싶으면 blank=True 로 설정해서 Null 값과 중복되지 않게 한다.
  * ~~null=True~~ , `blank=True`!!

---

브라우져 캐싱

- **웹 캐시는 자주 쓰이는 문서의 사본을 자동으로 보관하는 HTTP 장치다.** 웹 요청이 캐시에 도착했을 때, 캐시된 로컬 사본이 존재한다면, 그 문서는 원 서버가 아니라 해당 캐시로부터 제공된다.
- 캐싱의 장점
  - 캐시는 불필요한 데이터 전송을 줄여서, 네트워크 요금으로 인한 과금 걱정을 줄여 준다.
  - 네트워크 병목을 줄여 준다. 대역폭을 늘리지 않고도 페이지를 빨리 불러올 수 있게 된다.
  - 원 서버에 대한 요청을 줄여 준다. 서버에 부하가 줄어드니 더 빨리 응답이 가능해진다.
  - 페이지를 먼 곳에서 불러 올수록 시간이 많이 걸리는데, 캐시는 거리로 인한 지연을 줄여 준다.

