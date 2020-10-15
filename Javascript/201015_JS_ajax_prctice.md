#### 1015_practice 구현 설명



axios CDN을 위로 올려도되는데 html코드보다 위로 올라가면 성능상에 문제가 있을 수 있기 때문에 바디 제일 아래 `<script>` 태그에 `{% block script%}`를 선언하는게 좋다.



이런 식으로 

```html
(중략)
  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  {% block script %}
  {% endblock %}
  <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js" integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV" crossorigin="anonymous"></script>
</body>
</html>
```







구글링 django csrf token

https://docs.djangoproject.com/en/3.1/ref/csrf/



cookie에 저장된 csrf 토큰을 가져와서 사용하는 방식을 택하자.

Acquiring the token if CSRF_USE_SESSIONS and CSRF_COOKIE_HTTPONLY are False 일 때, 

즉 둘 중 하나라도 true가 된다면, csrf_token은 쿠기에 더이상 저장이 되지 않는다.

그러면 

Acquiring the token if CSRF_USE_SESSIONS or CSRF_COOKIE_HTTPONLY is True 일 경우에는 이렇게 가져올 수 있다.

```
{% csrf_token %}
<script>
const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value;
</script>
```



구글링 mdn 특성선택자 https://developer.mozilla.org/ko/docs/Web/CSS/Attribute_selectors

CSS 특성 선택자는 주어진 특성의 존재 여부나 그 값에 따라 요소를 선택합니다.

[attr=value]
attr이라는 이름의 특성값이 정확히 value인 요소를 선택합니다.







axios github

https://github.com/axios/axios

##### axios-post(url[, data[, config]])

```
  // `headers` are custom headers to be sent
  headers: {'X-Requested-With': 'XMLHttpRequest'},
```



그리고 Setting the token on the AJAX request¶ (https://docs.djangoproject.com/en/3.1/ref/csrf/)

```
 {headers: {'X-CSRFToken': csrftoken}}
```



최종적으로.

```javascript
  const headers = {
    headers: {
      'X-CSRFToken': csrftoken
    },
  }
  
  
axios.post('/articles/10/like/', {}, headers)
```

두 가지 공식문서 조합된 거니까 신중하게 보기!





mdn html data

https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/dataset

```
Accessing values
속성들은 element.dataset.keyname과 같이 dataset의 객체 속성처럼 camelCase 이름(키)을 사용해서 설정하거나 읽을 수 있습니다
속성들은 또한 element.dataset[keyname]과 같이 객체 속성의 괄호 신택스로 설정하거나 읽을 수 있습니다.
```

*정리*

`<img data-변수명="값">`

Javascript에서는  img.dataset.변수명

```
<form id="like-form" class="d-inline" data-pk="{{article.pk}}">
```

```
const articlePk = likeForm.dataset.pk
```

```
const icon = document.querySelector(`#like-${articlePk}`)
```





필요한 게 뭔지, 어떤 동작을 해야하는지 생각하면서 흐름 따라가기!

### Like 흐름

* 시작점. 이벤트 리스너를 달기
  * querySelectorAll
* axios 요청
  * get으로 시작해서 -> post로 요청 완성.
  * 각각의 form을 특정하기 위해서 article.pk로 dataset을 설정
  * csrf token 
* diango 응답을 수정
* .then으로 받아서 
  * 하트 변경
    * dataset으로 각각의 하트의 형태를 변경
  * 좋아요한 사람 수도 변경