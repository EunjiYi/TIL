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



