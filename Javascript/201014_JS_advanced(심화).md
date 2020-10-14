### JS 심화



ASI(Automatic Semicolon Insertion)



JS 코딩 스타일 가이드

구글링 javascript standard style

https://standardjs.com/rules.html



2 space

함수 선언 괄호 앞에 공백 추가

== (값비교) 대신 ===(값이랑 타입을 다 비교) 사용한다. 





### AJAX: 비동기통신

Asynchronous JavaScript And XML

계층형 데이터의 표준 형식

서버와 통신하기 위해 ` XMLHttpRequest`객체를 사용하는 것

JSON, XML, HTML, 일반텍스트 형식 등 다양한 포맷을 주고 받는다.

페이지 전체를 리프레쉬(=reload) 하지 않고서도 수행되는 비동기성

사용자의 이벤트가 있으면 `페이지(html가 아닌 일부분만 업데이트`



서버로부터 데이터를 받고 작업을 수행

페이지 새로고침 없이 서버에 요청



AJAX는 괜찮은 기숧들의 집약체.





### XHR(XMLHttpRequest)

서버와 상호작용하기 위해서 사용한다.

전체페이지의 새로고침 없이 URL로부터 데어터를 받아온다.

사용자가 하고 있는 것을 방해하지 않으면서 페이지의 일부를 업데이트 할 수 있다.

AJAX 프로그래밍에 주로 사용이 된다. 



------------



#### HOW JavaScript works (중요)

​	Asynchronous(비동기)

​	Single Thread( 혼자 일한다.)

​	Event Loop (혼자 일하는 방식)





구글링 jsonplaceholder typicode

https://jsonplaceholder.typicode.com/





기다려줌

```python
response = requests.get(url)
todo = response.json()
print(todo)
```



기다리지 않음 = 비동기

```javascript
const xhr = new XMLHttpRequest() // 요청했는데 기다려주지 않고
xhr.open('GET', url) // 바로 다음꺼 실행한다.
xhr.send()

const todo = xhr.response // 그래서 이거 찍어보면 아무것도 안나온다.
console.log(todo)
```

왜 안기다리는데? 혼자서 일을 하기 때문에. (=JS가 Single Thread 기반의 언어라서) 기다리면 그 뒤의 일을 다 못한다.

웹브라우저 console창에 while (true) {} 라고 치면 클릭,타이핑 등등 아무것도 안됨. 계속 무한루프 돌고있어서 = 혼자 일하니까.



Event Loop

```
console.log('Hi')

setTimeout(function ssafy () { // 요청을 따로 보내놓은 것은 나중에 따로 나온다. = 동작방식 알기!
	console.log('SSAFY 4th')
}, 3000) // 3000ms라서 3 - 3초뒤에 SSAFY 4th 찍어라.
console.log('Bye')
```

Hi

Bye

SSAFY 4th 

우리의 예상과 다르게, 이 순서로 찍힌다. 3초를 0으로 바꿔도 동일한 결과.



#### 이유

#### Event Loop



Call Stack : 요청이 들어올 때마다 해당 요청을 순차적으로 처리하는 스텍(LIFO)형태의 자룡구조 = 함수 호출을 기록한다.

Web API(Browser API): 자바스크립트가 엔진이 아닌 브라우저 영역에서 제공하는 API(=프로그래밍으로 하는 무언가) - `setTimeout`, AJAX -> `xhr`, `setInterval` 등 타이머 함수들이 이런 특징을 가진다. 

Task Queue: 콜백 함수가 대기하는 큐(FIFIO) 형태의 자료구조 = 함수가 대기

Event Loop: 스텍에 함수가 없으면 큐에 있는 함수를 스텍에 밀어넣는 친구. call Stack에서 현재 실행 중인 Task가 없는지 확인하고 Task Queue에 Task 가 있는지 확인.





### Event Loop 동작방식

```javascript
function func1 () {
    console.log('Hello')
    func2()
}

function func2 () {
    setTimeout(function () {
	    console.log('Eunji!')        
    }, 0)
	func3()
}

function func3 () {
    console.log('Bye')
}


func1()
```

996F87445BC7D90209.gif



#### 정리

1. AJAX - 비동기성,  reload x(리프레쉬 x, 새로고침 x)

   > 1요청 1응답은 새로고침 된다. 화면 최하단에 있는 좋아요를 누르는 순간 새로고침이 되서 화면 최상단으로 올라옴 - 사용자경험 안 좋다. `큰소리로 말하는 느낌`
   >
   > 그래서 비동기요청은 비동기 응답으로 받게 되면, 요청에 대한 응답만 받아지고 reload되지 않는다.  `속상여서 해당 부분만 조용히 가져오는 느낌`
   >
   > AJAX도 Response(요쳥)을 보내서  HTML / JSON/ XML 다 받을 수 있다.

2. 비동기(asynchronous) = 기다려주지 않아요. 왜 기다려주지 않는데요? = 나는 혼자서 일하니까요.
3. 그럼 어떻게 일을 하나요? = Event Loop에 기반한 일을 합니다.



기다려주면 안될까?

어떤 요청을 보냈을 때, 서버에서 몇 초/몇시간 만에 응답을 줄 지 보장할 수가 없다. 알 수 없어. 왜냐면 통제권이 나한테 없어. 외부에 요청하는 것은 그 서버가 얼마나 많은 트래픽, 성능 등등을 가지는지 알 수가 없다.

= 내가 언제 이 일을 완료할 수 있는지 알 수 가 없으니 `~~하면 ~~한다`고 걸어 두는것이다. = 콜백함수로 만든다.



-------------



### Callback function: 인자로 들어가는 함수, 전달된 함수는 특정조건이 되면 실행된다.



인자로 넘어가는 함수 = CallBack

>  함수를 return할 수 있다.

>  변수에 담을 수 있다.

>  함수를  인자로서 넘길 수 있다.



대표적 예시

django urls.py

path(패텬, `visws.index=여기가 callback`) 

// 특정한 조건이 만족했을 때 실행될 수 있도록 인자로 넣어주는 함수



callback hell

```
(질문에 대한 답장이 토착하면) 문제 해결
	(문제가 해결되면) 친구에게 알려주기
		(친구에게 알려주면) 반 전체에 공유해달라고 부탁하기
			(부탁하면) sns 채널에 공유하기
				(공유가 완료되면) 이모티콘 달기
					...
						콜백 함수의 늪...
```



---------



콜백으로 처리할 수 밖에 없는데 좀더 깔끔하게 할 순 없나?

### Promise(Promise 객체다) = 약속, 미래를 약속하는 객체 

> 미래의 성공 혹은 실패를 약속한다.

Promise 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과값을 나타낸다. Promise는 비동기 작업의 최종완료 또는 실패를 나타내는 객체다. 



#### .then(doSomething) // 성공했을 때 할 일 

> 성공상황은 resolve라는 콜백함수에 담아서 리턴 => 그리고 나서 then으로 처리

성공했을 때 리졸브함수에다가 응답을 인자로 넣어서 함수가 실행된 결과를 프로미스에 담아서 then에 전달되는 것. 그럼 반환된 promise object를 then이 받는다. 

```

```



#### .catch(doSomething) // 실패했을 때 할 일 

> 실패상황은 reject이라는 콜백함수에 담아서 리턴 => 그리고 나서 catch로 처리

성공했을 때 리졸브함수에다가 응답을 인자로 넣어서 함수가 실행된 결과를 프로미스에 담아서 then에 전달되는 것. 그럼 반환된 promise object를 then이 받는다. 





> then이 여러개 있으면 바로 이전then의 리턴값을 다시 받음
>
> ```javascript
> axios.get('https://jsonplaceholder.typicode.com/todos/1')
>       .then(function (res) {
>     	const article = JSON.parse(res)
>         return article.title
>       })
>       .then(function (res) {
>     	console.log(res)
>       })
> 	  .catch(function (error) { // error 잡아준다.
>         console.log(error)
>       })
> ```





이걸로 callback hell에서 벗어나자!

그래도 then -> then -> then 늪에 빠지는 건 마찬가지 = *Promise chaining*. 

그래서 async & await  등장. = 문법적인 설탕

-------



### async & await (ES8+에 등장) = 동기적으로 (보이도록) 코드를 쓰자!

자바스크립트의 최신 문법을 반영하는 회사가 많아지고 있다.

Syntactic Sugar 문법적 설탕 = 문법이 너무 어렵다/ 복잡해서 사용하기 x . 그래서 내부적인 동작은 그대로 가되 사용하기 쉽게 만든 것.



async function

> async와 await가 가능하게 해준다. Promise 베이스인데 `조금 더 cleaner한 스타일`, chaining (체이닝)이 너무 많아서 막아주기 위해, 더 나아가서 코드를 동기적으로 사용하기 위해 async & await  쓴다. 

비동기적으로 동작하는 함수 앞에 await (=비동기 요청을 받으면 기다려라)를 붙인다. (await는 async 키워드 안에서 사용가능.)

> 동기처럼 보이게 하는 거지, 동작은 비동기다.



### 지금까지의 흐름 정리

비동기적으로 처리하기 위해서 콜백함수 사용 => 사용하다가 콜백 헬(콜백이 꼬리의 꼬리를 무는 것)이 발생 -> Promise(ES6에 등장)로 성공/실패를 약속해서 성공하면 .then이라고 하는 것 안에 콜백으로 / 실패하면 .catch안에 콜백으로 -> 이것도 chaining으로 복잡해 질 수 있다. -> 실제 내부는 비동기 적으로 처리되지만` 겉으로 보기에는 동기적으로 처리되는 것`처럼 보인다. async / await : 복잡성을 줄이고 동기적으로 보이게 한다.



----------

----------------



지금까지는 Axios라는 자바스크립트패키지를 사용하기 위한 buiil up이었다.

결국에 우리는 Axios를 잘 사용하면 된다.!

ajax 통신(=비동기요청)을 axios(=하나의 도구)로 구현하는 거다.

### Axios (Promise based HTTP client for the browser and node.js)

> 요청할 때 쓰는 라이브러리
>
> 프로미스 기반으로 요청을 날려주는 친구다. 
>
> Axios를 이용해서 비동기요청을 Promise 기반으로 처리한다.
>
> 

파이썬의 request와 비슷한 친구



편하게(직관적으로) 요청을 보내자. Promise의 기반해서(그러니까 .then과 .catch 가 가능)



axios.get( )

post( ) 도 가능

```javascript
axios.get('https://jsonplaceholder.typicode.com/todos/1')
      .then(function (res) {
        console.log(res.data.title)
      })
	  .catch(function (error) { // error 잡아준다.
        console.log(error)
      })
```





구글링 axios github

https://github.com/axios/axios

Using jsDelivr CDN:

```
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

적용됐는지 확인

크롬 F12키 개발자도구 Network 탭에 보면 axios.min.js 있다.





chaining이란 = 연결

```javascript
// chaining
axios.get('https://jsonplaceholder.typicode.com/todos/1')
      .then(function (res) {
        console.log(res)
        return res.data
      })
      .then(function (res) {
        console.log(res)
        return res.title
      })
      .then(function (res) {
        console.log(res)
      })
      .catch(function (err) { // error 잡아준다.
        console.log(err)
      })
// 앞의 then에 return 없어도 뒤에 then 동작함
```

 

----------



### 실습

python manage.py seed articles --number=20

seed가 똑똑한게, article models.py class 중에 like_user 필드가 User 모델을 참조한다. 그래서 User 의 더미데이터 까지 같이 만든다.





요청을 보내고, JSON 데이터를 받아서 이걸 토대로 DOM을 조작해서 좋아요하트의 색깔을 변경한다.

> 요청의 횟수도 줄고, reload도 일어나지 않는다. 



ajax

https://ko.wikipedia.org/wiki/Ajax



axios는 비동기로 get/post 요청을 보낼 수 있다.







data attribute html

https://developer.mozilla.org/ko/docs/Learn/HTML/Howto/%EB%8D%B0%EC%9D%B4%ED%84%B0_%EC%86%8D%EC%84%B1_%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

`dataset` 객체를 통해 `data` 속성을 가져오기 위해서는 속성 이름의 `data-` 뒷 부분을 사용합니다.(대시들은 camelCase로 변환되는 것에 주의하세요.) 





django ajax request documentation

https://docs.djangoproject.com/en/3.1/ref/csrf/

```
While the above method can be used for AJAX POST requests, it has some inconveniences: you have to remember to pass the CSRF token in as POST data with every POST request. For this reason, there is an alternative method: on each XMLHttpRequest, set a custom X-CSRFToken header (as specified by the CSRF_HEADER_NAME setting) to the value of the CSRF token. This is often easier because many JavaScript frameworks provide hooks that allow headers to be set on every request.
```





ajax github

https://github.com/axios/axios

```
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```





id attribute start with

https://stackoverflow.com/questions/70579/what-are-valid-values-for-the-id-attribute-in-html

ID and NAME tokens must begin with a letter 

