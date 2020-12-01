static



settings.py

static url ->  스태틱파일로 접근할 수 있는 경로

static root -> 배포시 모든 이미지를 한군데에 모아줌

staticfiles_dirs -> 특정 앱이 아닌 전체 지정이 필요할 때.

  <img src="{% static 'boards/asdf4321.jpg' %}" alt="test">

  <img src="{% static '1234fdsa.jpg' %}" alt="test123">





media

pip install Pillow

image = models.ImageField(upload_to="%Y/%m/%d/", blank=true, null=true)



settings.py

MEDIA_URL -> 안세익도 정확히는 모르지만, 사용자가 올린 이미지를 접근할 수있는(=볼 수 있는) 가상의 경로.  `<img src="/media/2020/12/01/asdf4321.jpg" alt="media">` 

사실 이렇게 접근할 일은 없다. 사용자가 무슨 이름으로 올렸을 줄 알고 하드코딩하느냐.



MEDIA_ROOT -> 사용자가 올린 이미지가 저장되는 경로. upload_to를 지정하면 이 폴더 아래에 생긴다. 





views.py

form = BoardForm(request.POST, request.FILES) # 이미지를 포함한 모든 form 인스턴스 생성



new.html

post요청 시 

`<form 속성에 method = "POST"  enctype="mutipart/form-data">`



프로젝트 urls.py

from django.conf import settings

from django.conf.urls.static import static

`+ static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT) 지정



detail.html

<img src = {{  views.py의 detail 함수에서 받아온 인스턴스 명.이미지필드명.url }}

`{{ data.image.url }}`



Done.