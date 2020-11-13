# Vue Directive

```html
<div id="app"></div>

<!-- Vue CDN 추가-->
<script>
	const app = new Vue ({
        el: '#app', // 어떤 엘리먼트와 연결할지 정함.
        data: {
            // Vue에서 사용되는 변수들
            // 다양한 정보의 타입이 저장될 수 있다.
        },
        methods: {
            // methods의 s를 빼먹지 않도록 주의!!
            // Vue에서 사용할 함수들을 정의하는 곳
            // 메소드를 정의할 때는 화살표 함수를 사용하지 않는다.
            // 화살표 함수는 자신의 this가 없고 화살표 함수를 둘러싸는 렉시컬 범위의 this가 사용된다.
        }
    })
</script>
```

* v-html

  * innerHTML로 할당
  * HTML을 그대로 읽기 때문에 XSS 공격에 취약하다.

* v-text

  * innerText로 할당
  * {{ mustache }}: 보간법 (interpolation)과 동일한 역할

* v-if, v-else-if, v-else

  * 조건문에 따라서 해당 Tag의 렌더링 여부를 결정
  * 해당 조건에 부합하지 않으면 코드 자체가 렌더링 되지 않는다.
  * v-if, v-else-if, v-else를 사용할 때 사이에 다른 Tag가 있으면 제대로 동작하지 않는다.
  * 렌더링 비용은 낮고 토글 비용은 높다.(한번 렌더링하고 다시 렌더링하지 않아도 될 경우에 사용)

* v-show

  * v-show의 값에 따라 display 속성을 조절해서 화면 노출을 결정
  * 코드는 렌더링되지만 화면에 보여줄지 보여주지 않을지 결정
  * 렌더링 비용은 높고 토글 비용은 낮다.(반복적으로 토글되야하는 상황에서 사용)

* v-for

  * 반복문

* v-bind

  * HTML 표준속성에 Vue의 데이터를 연결

  * `:`(shortcut)

  * Object 형태(key-value)로 사용하면 value가 true인 경우만 바인딩 된 값으로 할당 가능.

    `:class="{클래스명: true}"`

  * 배열 형태로 사용도 가능

    `:class="[클래스명1, 클래스명2]"`

* v-model

  * 양방향 바인딩
  * 입력하는 태그에서 사용(input, textarea, select)

* v-on

  * 이벤트
  * `@`(shortcut)

* this 정리

  * obj.functionCall() => this === obj : 메소드 호출되었을때

  * 그 외 => this === window

    ```js
    const myObj = {
        myFunc: function () {
            console.log(this) // myObj
            // 1. 콜백 함수에서 this를 obj로 만드는 방법(.bind)
            axios.get(URL)
    	        .then(function () {
                console.log(this) // window
            }.bind(myObj)) // .bind(myObj)를 사용하면 this가 myObj를 의미한다.
            
            // 2. 콜백 함수에서 this를 obj로 만드는 방법(화살표 함수 사용)
            axios.get(URL)
            	.then(() => {
                console.log(this) // myObj
            })
        }
    }
    ```



## computed & watch

### Computed

* 값을 캐싱하기 때문에 값이 변하지 않으면 기존에 계산된 값(캐싱된 값)을 사용
* x + 2 (x=1) = 3
  * x값이 변하지 않으면 기존에 계산된 3을 계속 사용한다.
  * x 값이 2로 변하면 다시 계산을 하여 4를 저장한다.
* 특정한 데이터를 직접적으로 가공해여 다른 값으로 만들어 사용할 때 주로 활용
  * `'반갑습니다. 00시 입니다.'`
* 최종 데이터형식이 정해져 있고 변경된 값은 항상 최종 데이터 형식을 가지기 때문에 `선언형 프로그래밍`이라고 한다.



### Watch

* 데이터가 변경이 되는지 지켜보고 변경이 된다면 특정 함수를 실행.
* 특정한 데이터의 변화에 따라서 다른 데이터 혹은 환경 등을 변화시켜야 하는 경우에 주로 활용
  * ``00시의 날씨는 00입니다.`

* `명령형 프로그래밍` : 변경된 데이터가 있으면 추가적인 작동을 지시한다.

D2Coding, Consolas, 'Courier New', monospace



, D2Coding, Consolas, 'Courier New', monospace