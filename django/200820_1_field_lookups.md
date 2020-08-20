### Field lookups



구글 django queryset -> 처음꺼 -> 오른쪽 카테고리바 field lookups 클릭 

https://docs.djangoproject.com/en/3.1/ref/models/querysets/#field-lookups



`필드명__필드룩업` 형태로 사용한다.

* exact: 대소문자까지 전부 일치해야함.

* iexact: 대소문자는 상관없이 일치해야함.

* contains: 해당 글자가 포함되어 있어야함. (어느 위치이던 상관 ㄴㄴ 그냥 포함만 되어 있으면 됨)

* startswith: 해당 글자로 시작하는 것만

* endswith: 해당 글자로 끝나는 것만

* gt / gte / lt / lte

  

python manage.py shell_plus 키고

#### 실습시작

* 제목이  first이고, 한 개만 가져와라. = 여러개의 데이터가 있는데 하나만 가져오고 싶을 때

```
Article.objects.filter(title="first").first()

```

In [2]: Article.objects.filter(title="first")
Out[2]: <QuerySet [<Article: first>]>  -> 반환타입 `쿼리셋`



In [1]: Article.objects.filter(title="first").first()
Out[1]: <Article: first> -> 반환타입 `해당 모델의 클래스로 값이 리턴`된다. 





* 정렬을 하고 싶을 때 (오름차순, 내림차순)

```
#오름차순
Article.objects.order_by('pk')

#내림차순
Article.objects.order_by('-title')
```





* QuerySet으로 리턴을 받았을 때

  * QuerySet은 List와 유사하다. 그래서 indexing이 되고 slicing도 가능하다.

    ```
    # indexing
    Article.objects.all()[2] -> 이러면 3번째 값(index = 2) 반환해줌.
    그런데 마이너스 인덱싱은 불가능하다.
    Article.objects.all()[-1] -> Negative indexing is nor supported.로 AssertionError 발생
    ```

    

  ```
  # slicing
  Article.objects.all()[:2] > 1,2번째만 나옴. (index = 0, 1)
  ```

  



