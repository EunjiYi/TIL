### JS 심화



ASI(Automatic Semicolon Insertion)



JS 코딩 스타일 가이드

구글링 javascript standard style

https://standardjs.com/rules.html



2 space

함수 선언 괄호 앞에 공백 추가

== (값비교) 대신 ===(값이랑 타입을 다 비교) 사용한다. 





### AJAX

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



#### HOW JavaScript works

​	Asynchronous(비동기)

​	Single Thread

​	Event Loop





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
const xhr = new XMLHttpRequest()
xhr.open('GET', url)
xhr.send()

const todo = xhr.response
console.log(todo)
```

왜 안기다리는데? 혼자서 일을 하기 때문에. (=JS가 Single Thread 기반의 언어라서) 기다리면 그 뒤의 일을 다 못한다.

웹브라우저 console창에 while (true) {} 라고 치면 클릭,타이핑 등등 아무것도 안됨. 계속 무한루프 돌고있어서 = 혼자 일하니까.



Event Loop

```
console.log('Hi')

setTimeout(function ssafy () {
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



#### 정리

1. AJAX - 비동기성,  reload x(리프레쉬 x, 새로고침 x)

> AJAX도 Response(요쳥)을 보내서  HTML / JSON/ XML 다 받을 수 있다.

2. 비동기(asynchronous) = 기다려주지 않아요. 왜 기다려주지 않는데요? = 나는 혼자서 일하니까요.
3. 그럼 어떻게 일을 하나요? = Event Loop에 기반한 일을 합니다.



기다려주면 안될까?

어떤 요청을 보냈을 때, 서버에서 몇 초/몇시간 만에 응답을 줄 지 보장할 수가 없다. 알 수 없어. 왜냐면 통제권이 나한테 없어. 외부에 요청하는 것은 그 서버가 얼마나 많은 트래픽, 성능 등등을 가지는지 알 수가 없다.

= 내가 언제 이 일을 완료할 수 있는지 알 수 가 없으니 `~~하면 ~~한다`고 걸어 두는것이다. = 콜백함수로 만든다.



### Callback function

인자로 넘어가는 함수 = CallBack

>  함수를 return할 수 있다.

>  변수에 담을 수 있다.

>  함수를  인자로서 넘길 수 있다.



```
(질문에 대한 답장이 토착하면) 문제 해결
	(문제가 해결되면) 친구에게 알려주기
		(친구에게 알려주면) 반 전체에 공유해달라고 부탁하기
			(부탁하면) sns 채널에 공유하기
				(공유가 완료되면) 이모티콘 달기
					...
						콜백 함수의 늪...
```



콜백으로 처리할 수 밖에 없는데 좀더 깔끔하게 할 순 없나?

Promise(객체) = 약속 

> 미래의 성공 혹은 실패를 약속한다.

Promise 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과값을 나타낸다. Promise는 비동기 작업의 최종완료 또는 실패를 나타내는 객체다. 



.then(doSomething) // 성공했을 때 할 일 => 성공상황은 resolve라는 콜백함수에 담아서 리턴 -> 그리고 나서 then으로 처리

.catch(doSomething) // 실패했을 때 할 일 => 실패상황은 reject이라는 콜백함수에 담아서 리턴 -> 그리고 나서 catch로 처리

이걸로 callback hell에서 벗어나자!



-------



### async & await (ES8+에 등장)

자바스크립트의 최신 문법을 반영하는 회사가 많아지고 있다.

Syntactic Sugar 문법적 설탕 = 문법이 너무 어렵다/ 복잡해서 사용하기 x . 그래서 내부적인 동작은 그대로 가되 사용하기 쉽게 만든 것.



async function

> async와 await가 가능하게 해준다. Promise 베이스인데 `조금 더 cleaner한 스타일`, chaining (체이닝)

비동기적으로 동작하는 함수 앞에 await 를 붙인다. (await는 async function 안에서 사용가능.)



### 지금까지의 흐름 정리

비동기적으로 처리하기 위해서 콜백함수 사용 => 사용하다가 콜백 헬(콜백이 꼬리의 꼬리를 무는 것)이 발생 -> Promise(ES6에 등장)로 성공/실패를 약속해서 성공하면 .then이라고 하는 것 안에 콜백으로 / 실패하면 .catch안에 콜백으로 -> 이것도 chaining으로 복잡해 질 수 있다. -> 실제 내부는 비동기 적으로 처리되지만` 겉으로 보기에는 동기적으로 처리되는 것`처럼 보인다. async / await : 복잡성을 줄이고 동기적으로 보이게 한다.





### Axios

> 요청할 때 쓰는 라이브러리

파이썬의 request와 비슷한 친구

Promise based HTTP client for the browser and node.js

편하게(직관적으로) 요청을 보내자. Promise의 기반해서(그러니까 .then과 .catch 가 가능)



