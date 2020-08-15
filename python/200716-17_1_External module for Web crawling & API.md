### **웹크롤링을 위한 파이썬 외장모듈 & API**





https://openweathermap.org/current#name

날씨 정보에 관한 api를 제공함.



-------------

**웹 크롤링 & API 통신의 큰 흐름**

1. url로 요청을 한다.

2. 받은 응답을 가지고 원하는 데이터를 가지고 온다.

----------



**외장 모듈 2가지의 종류**

* 파이썬이 기본으로 제공하는 외장모듈 (책상 서랍에 위치)
  * import
  * 사용
* 다른 사람이 만들어둔 외장모듈 (문구점에 사러가야 함.)
  * pip 툴을 이용해서 외장 모듈을 설치
  * import
  * 사용



--------------

**pip : 파이썬 패키지 관리툴**



**웹크롤링을 위한 외장모듈**

1. requests

   - 간편한 http 요청 처리기가 들어 있는 모듈.
   - 설치 하는 방법 `pip install requests`

2. BeautifulSoup

   - 텍스트로 나타나는 html 을 우리가 사용하기 쉽게 바꿔주는 역할을 하는 모듈.

   - 설치가 필요함. `pip install beautifulsoup4`

   - 파이썬 내장 함수인 json 을 활용해서 json 형태는 -> Dictionary 형태로 변환해서 사용.

     > from bs4 import BeautifulSoup
     >
     > data = BeautifulSoup(response, 'html.parser')

------------



코스피 초안

```python
import requests
url = 'https://finance.naver.com/sise'

response = requests.get(url).text

print(response)
```


