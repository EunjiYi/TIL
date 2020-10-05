HTTP는 **마크업랭귀지**다. 즉, **''구조+데이터''가 포함된 언어**다. 

그런데 이 데이터를 안드로이드,WINDOW,MAC 등 다양한 브라우저/Application/web/app 등에서/front-end에서 추출하려면 **구조**는 필요없음. **데이터**만 필요하다. 





JSON이 문자열(**문자열**로 이루어진 **키-값의 쌍** 모양)이라 바로 사용이 어려운 것. 

1, '1' -> 이건 바로 사칙연산 안되잖아.

**문자열을 딕셔너리** 형태로.



**RESTful은 MTV에서 T를 빼고 M V만 있는 거라 생각하면 된다.**

**T가 HTML인데, Response JSON으로 보여주니까 이 부분이 필요없음.**



기존(HTTP)

USER  --- 요청 ---> SERVER ( MTV )

​         <--응답(HTML)--



JSON

def create(request): 에서 request가 호출되는 요청

USER  --- 요청(data) ---> SERVER ( M V )

​         <--응답(JSON)--

이렇게 받아온 data 덩어리를 Java, C#, python 등에서 처리가 가능하다.

**입력창은 브라우저에서 알아서 보여주고 우리는 값만 받는다.**

HTTP처럼 제목적을 칸, 내용적을 칸 보내주고, 사용자가 입력하면 데이터 받아서 출력해주고 이런 과정 ㄴㄴ



* API POST 요청

  request.method == POST

  1. 값을 저장
  2. 저장된 값을 다시 보내준다.



---

### 멱등성 

GET 요청 2번 동일하게

PUT/POST/DELETE 첫번째와 두번째 요청이 동일하지 X

-> 동일한 요청이 2번 들어오는 것을 멱등성이라 한다.



# REST API

### API 서버

* 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스
* 결과적으로는 우리가 만든 서버에서 HTML이 아니라 **JSON응답으로 데이터를 제공**.



## RESTful API

### REST

* REpresentational State Transfer

* 로이 필딩 2000년 박사 논문으로.

* 자원과 주소로 표현하는 방법론

  * 웹 상에 존재하는 자료를 **HTTP 위에서 전송하기 위한 인터페이스**
  * 네트워크 아키텍쳐

  

* 자원(URI로 표현)

  * URL
  * URN

  

* HTTP method로 주소를 표현

  * **GET: 데이터를 조회**
  * **POST: 데이터를 생성**
  * PUT/ PATCH: 데이터를 수정
  * DELECT: 데이터를 삭제

  

* HTTP method + 자원(URI)의 표헌.

```
# HTTP method + URI
GET article/1/
POST article/
PUT article/1/
DELETE article/1/
=> urls.py 동일패턴, 1개의 함수에서 GET, POST, PUT, DELECT 하기.
```



_____

## Django Rest Framework (DRF)

* Serialization (직렬화)
  * 데이터 구조나 오브젝트 상태를, 동일하거나 다른 컴퓨터환경에 저장하고 나중에 재구성할 수 있는 포맷으로 변환하는 과정.
  * Django에서 Form을 작성하는 것과 굉장히 유사.

