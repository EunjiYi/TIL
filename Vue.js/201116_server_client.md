 server  폴더
django-admin startproject config
python manage.py startapp todos
settings.py 기본 셋팅
url분리
models.py 만들기
python manage.py makemigrations
python manage.py migrate

django restframework 이용하기
pip install djangorestframwork
installed_app에 등록

serializers.py 작성
from rest_framework import serializers
from .models improt Todo
class TodoSerializer(serializers.ModelSerializer):
class Meta:
model = Todo
fields = ['id', 'title', 'completed',] #'__all__'


client  폴더
vue create client