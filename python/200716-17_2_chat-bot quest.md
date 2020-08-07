http://py.hphk.io/

아이디 : eunji yi (띄어쓰기 대소문자 주의)

비번은 여기에 안 적을 거지롱

https://py.hphk.io/bots



#### 안녕

```python
greeting = '은지님 안녕하세요.'  # 그래 안녕하마
n=0
while n<7:
  print(greeting)
  n=n+1
  
print('--------------')  
  
for i in range(5): # {0,1,2,3,4} 
  print(greeting) 
```



#### 점심메뉴

```python
import random
# 1. menu 리스트를 만드세요.
menu = ["된장찌개", '보쌈','샌드위치', '막국수', '돼지고기김치찌개']
print(menu)
print(menu[1])
print(menu[0])


# 2. 전화번호부를 만드세요.
store = ['버거킹' ,'중화반점' , '비비큐' , '도미노' ] 
tel = ['23-23', '2435-4342', '43645-34234' , '65645634-34234234' ]
print(store , tel)
store_phone = { '버거킹' : '23-23' , '중화반점' : '23425-23425', '도미노' : '234234-234234', '비비큐' : '876678-765756' }
choice = random.choice(store)
print('=====')
print(choice)
print('=====')
print(store_phone[choice])
```





#### 미세먼지

```python
import requests
from bs4 import BeautifulSoup
url = f'http://openapi.airkorea.or.kr/openapi/services/rest/ArpltnInforInqireSvc/getCtprvnRltmMesureDnsty?serviceKey={key}&numOfRows=10&pageNo=1&sidoName=서울&ver=1.6'
response = requests.get(url).text
soup = BeautifulSoup(response, 'xml')
item = soup('item')[0]
time = item.dataTime.text          # 기준시간
station = item.stationName.text    # 위치
dust = int(item.pm10Value.text)    # 미세먼지 농도

# 위의 코드는 수정하지 마세요.

# 아래에 코드를 작성하세요.
# 1. dust 변수의 값을 출력하세요.
print('지금 시각은 {}'.format(time))
print(f'지금 시각은 {time} 이며 {station}의 미세먼지 농도는 {dust} 입니다.')

# 2. dust 변수의 값을 기준으로 상태 정보를 출력하세요.
if dust > 150:
	print('매우나쁨')
elif 150 >= dust > 80:
	print('나쁨')
elif dust > 30:
	print('보통')
else:
	print('좋음')
```





####  로또

```python
# 1. 필요한 모듈을 불러오세요.
import random

# 2. 1~45까지 숫자를 numbers에 저장하세요.
numbers = range(1,46) # 유사 리스트로 활용할 수 있는 것.
print(numbers) #이렇게 하면 range(1,46)이라 나옴. - 말그대로 걍 '유사'리스트

# 3. numbers 중에 6개의 숫자를 뽑아 lucky에 저장하세요.
lucky = random.sample(numbers,6)

# 4. lucky를 출력하세요.
print(lucky)
```





#### 코스피

```python
# 1. 필요한 모듈을 불러오세요.
import requests
from bs4 import BeautifulSoup

# 2. requests 모듈로 요청을 보내세요.
url = 'https://finance.naver.com/sise'
response = requests.get(url).text

# 3. bs4 모듈로 데이터를 가져오세요.
data = BeautifulSoup(response, 'html.parser')
select = data.select_one('#KOSPI_now')
print(select.text)
```





#### 날씨

```python
# 아래에 코드를 작성하세요.

# 1. 필요한 모듈을 불러오세요.
import requests

# 2. 요청 url을 만드세요.
city = 'Gumi'
url = f'https://api.openweathermap.org/data/2.5/weather?q={city}&appid={key}'
response =requests.get(url).json()

#기온에 대한 정보
data = response['main']
print(data['temp'])

# 3. 날씨, 현재온도, 최저 및 최고온도에 대한 데이터를 꺼내세요.
weather = response['weather'][0]['main'] #list라서 [0]필요
temp = data['temp'] - 273.15
max_temp = data['temp_max'] - 273.15
min_temp = data['temp_min'] - 273.15  #Celsius 변환


# 4. 꺼낸 데이터 값을 기준으로 상태 정보를 출력하세요.
print(f'현재 {city}의 날씨는 {weather}이며, 현재 기온은 {temp} (최고: {max_temp} / 최저: {min_temp})')
```

