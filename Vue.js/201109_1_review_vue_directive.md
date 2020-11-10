### Vue Directive

```html
<div id="app"></div>

<!-- vue CDN 추가 -->
<script>
	const app = new Vue({
        el: '#app', // 어떤 엘리먼트와 연결할지 정한다.
        data: {
            // vue에서 사용되는 변수들
            // 다양한 정보의 타입이 저장될 수 있다. 
        },
        methods: {
            // vue에서 사용할 함수들을 정의하는 곳
            // 메소들를 정의할 때는 화살표함수를 사용하지 않는다. (this 때문 = window를 가리키기 때문에!)
            // 화살표함수는 콜백함수에서만 사용한다! 메소드 정의시에는 사용하지 않는다.
        }
    })
</script>
```

* v-html
  * innerHTML 로 할당되는 값
  * HTML을 그대로 읽기 때문에 XSS 공격에 취약하다. 
* v-text
  * innerText로 할당
  * `{{ 머스타치 }}` : 보간법(interpolation) 과 동일한 역할
* v-if, v-if-else, v-else
  * 조건문에 따라서 해당 Tag의 렌더링 여부를 결정
  * 아예 코드 자체가 렌더링 되지 않는다. 
  * 렌더링을 안한다면 렌더링 비용이 절감되지만 반대로 여러번 바뀌어야하는 토글 비용은 올라간다.
  * v-if, v-else를 사이에 어떠한 Tag도 있어서는 안된다. 있으면 작동하지 않는다. ex. `<br>`태그 하나만 있어도 vi-if와 v-else가 동작하지 않는다.
* v-show
  * v-show의 값에 따라 css display 속성을 조절해서 화면 노출을 결정
  * `display: none`
  * 토글링 비용이 저렴하다. 그래서 개발시 화면전환이(토글이) 잦다면, 이 방법을 택하자.

* v-for

  * 반복문
  * vue.js v-for 구글링 https://kr.vuejs.org/v2/guide/list.html

* v-bind

  * HTML 표준 속성에 Vue의 데이터를 연결해주는 역할을 한다. 

  * shortcut `:`

  * Object 형태( key - value 형태)로 사용하면, value가 true인 경우만 바인딩된 값으로 할당 가능.

    `:class = "{ 클래스 이름:false }"`

*  v-model

  * 양방향 바인딩
  * 입력이 되는 값이 자동적으로 data에 입력되고, data가 수정되면 자동적으로 v-model에 잡힌 태그에 적용이 됨 `양방향 사용가능`
  * 입력되어지는 태그 (Input, TextArea, Select) 사용

* v-on
  * 이벤트
  * shortcut `@`



* this 정리

  * 메소드가 호출되었을 때 `obj.functionCall() 일 때`,  this가 obj와 동일

  * 그 외의 this는 window와 동일

    ```javascript
    const myObjg = {
    	myFunc: function () {
    		console.log(this) //myObj
            // 1. 콜백함수에서 this를 obj로 만드는 방법 (.bind)
    		axios.get(URL)
            	console.log(this) // window -> bind 적을 경우 -> myObj
        	}.bind(myObj)) // 이걸 적을 경우 window -> myObj
        	
        
        	// 2. 콜백 함수에서 this를 obj로 만드는 방법
        	axios.get(URL)
            	.then(() => { //arrow function 사용하면 된다. method에서 호출한 this를 가져온다. 그래서 앞으로 axios 쓸 때는 화살표함수로 선언하겠다.
                    console.log(this) // myObj
            })
    	}
    }
    ```

    

-----



### computed & watch

구글링 vue computed watch

https://kr.vuejs.org/v2/guide/computed.html



### computed

1 + 2라는 수식이 아닌 결과값인 3으로 메모리에 캐싱해 놓고 필요할 때마다 3을 가져다 쓴다.

* 값을 캐싱하기 때문에 값이 변하지 않으면 기존에 계산된 값(캐싱된 값)을 사용한다.
* x + 2 (x = 1) -> 3
* x + 2 (x = 2) -> 4
* x의 값에 따라 결과가 달라지는데(의존성있는 값), 이 의존성있는 값이 변하지 않으면 캐싱된 값을 사용한다.
* 의존성 있는 값이 변하면 계산해서 바뀐 값을 저장해둔다.

* **특정한 데이터를 직접적으로 가공하여 다른 값으로 만들어 사용할 때 주로 활용**

  `반갑습니다. 00시 입니다.` - 지역만 바뀌면 됨.

* 최종 데이터 형식이 정해져 있고 변경된 값은 **항상 최종 데이터 형식**을 가지기 때문에 `선언형 프로그래밍` - 답정너



### watch

* 데이터가 변경이 되는지 지켜보고 `변경이 된다면` 특정 함수를 `실행한다.`

* **특정한 데이터의 변화에 따라서 다른 데이터 혹은 환경 등을 변화시켜야 하는 경우에 주로 활용**

  `00시의 날씨는 00입니다.`- 각 지역에 대해서 특정한 데이터 변화가 필요할 때 = 지역이 변화하면 날씨도 변화하니까.

  https://kr.vuejs.org/v2/api/#vm-watch

* `명령형 프로그래밍` - 시키는 일



