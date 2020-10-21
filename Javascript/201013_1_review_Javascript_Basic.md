### DOM 조작

* HTML을 JS로 조작

  * window = 브라우저 전체
  * document - 화면에 보여지는 문서
  * elements - document 안에 있는 요소들

* selector를 이용해서 조작

  * `querySelector` / `querySelectorAll`
  * dir( 선택된 엘리먼트를 가진 변수 )
    * 사용할 수 있는 속성 정보를 볼 수 있다.
    * mdn 문서(mdn + 찾으려는 속성 정보로 구글링하기)

* DOM 조작 정리

  1. 선택한다.
  2. 수정 및 변경한다. =  조작한다.

  

-------

 ### 이벤트 리스너

* 이벤트는 브라우저에서 벌어지는 일

* 특정 이벤트가 떨어지면 특정 행동을 한다. `~하면 ~한다.`

  `이벤트타겟 addEventListner(이벤트타입, 할 일)`

* preventDefault()
  
  * 기본 동작을 동작하지 않게 막을 수 있다.

---------------



JSON은 JavaScript Object Notation의 약자다.

.json 확장자를 가진 단순 텍스트 파일에도 저장이 가능하다.

문자열, 숫자, 배열, 불리언 등의 형식을 가진 데이터를 구축할 수 있다.



변환 object -> JSON (string)`JSON.stringify(objData)`

JSON -> object  `JSON.parse(jsonData)`

----

preventDefault() <- 괄호 있어야한다.



문자열 표현은 `''` `""` 그리고 `백틱`



Promise 객체는 .then 메소드를 통해 Promise가 이행된 경우의 실행할 함수를 정의한다.

.then에 .catch를 연결해서 바로 사용할 수 있는 이유는 .then의 반환값이 Promise 객체라서 가능하다.

Promise 객체는 .then 메소드를 통해 Promise가 이행된 경우에 실행할 함수를 정의한다.

Promise 객체는 .catch 메소드를 통해 Promise가 거부된 경우에 실행할 함수를 정의한다. 



브라우저 **전역**객체는 window

-------

변수 선언시 

`$` `밑줄 _` `문자`로 시작해야한다. `숫자`와 `- 하이픈`는 안 됨.



album.songs.find(function (song, index) {

​	return index === 2

})