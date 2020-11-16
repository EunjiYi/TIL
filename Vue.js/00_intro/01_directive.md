# 01_Directive

> 2020.11.09 오후 라이브

[강의 코드](https://lab.ssafy.com/ssafy4/vue)



강의 및 webex 시간에 활용했던 코드를 중심으로 살펴보시면 학습에 도움이 될 거에요. 😊

추가적으로 공식 링크를 걸어 놓을테니 문서에 나와있는 추가적인 내용도 한번씩 사용해보세요. 😁



## 핵심

### 1. Vue Instance

https://kr.vuejs.org/v2/guide/instance.html

1. 가장 기본 요소이자 Vue의 중심이 되는 요소
2. Vue Instance는 ViewModel이다.
3. View와 Model 사이의 중개자 역할을 한다.



### 2. Vue Data Binding

https://kr.vuejs.org/v2/guide/syntax.html#%EB%B3%B4%EA%B0%84%EB%B2%95-Interpolation

1. data object는 Vue Instance 내부에 들어가면 DOM과 반응형으로 연결된다. 
2. 데이터 바인딩의 가장 기본 형태는 콧수염 태그(`{{ }}`)를 사용한 텍스트 보간법이다.
3. DOM에서 `{{ }}` 태그를 활용해 data의 특정 속성값을 보여줄 수 있다. 
4. data가 변화하면 DOM이 다시 렌더링된다.



### 3. `v-for`

https://kr.vuejs.org/v2/guide/list.html

1. 기본적인 반복문으로 사용한다. 
2. `key` 설정이 필요하다.
   - 수요일 진행 예정



### 4. `v-if `

https://kr.vuejs.org/v2/guide/conditional.html

1. 기본적인 조건문으로 사용한다.
2. `v-else-if`와 `v-else` 구문을 사용할 수 있다.
   - <u>`v-else`는 반드시 `v-if` 디렉티브 뒤에 사용해야 한다.</u>
3. `v-show`와 비교하여 필요한 상황에 더 적절한 디렉티브를 사용하면 된다.



### 5. `v-bind`

https://kr.vuejs.org/v2/guide/class-and-style.html

1. HTML 표준 속성 바인딩
2. class 바인딩
3. 하위 컴포넌트에게 데이터를 내려 줄 때 활용
   
- 수요일 진행 예정
   
4. shortcut 

   https://kr.vuejs.org/v2/guide/syntax.html#v-bind-%EC%95%BD%EC%96%B4

   - `v-bind` -> `:`



### 6. `v-on` & methods

https://kr.vuejs.org/v2/guide/events.html

`v-on`

1. 이벤트 리스너의 역할을 하는 디렉티브

2. `.`을 통해 여러 이벤트를 연결할 수 있다.

3. shorcut

   https://kr.vuejs.org/v2/guide/syntax.html#v-bind-%EC%95%BD%EC%96%B4

   - `v-on` -> `@`



methods

1. Vue에서 활용하는 기본적인 함수를 정의하는 곳이다.



### 7. `v-model`

https://kr.vuejs.org/v2/guide/forms.html

1. 폼 input과 textarea 그리고 select와 같은 HTMl Element에 양방향 데이터 바인딩을 할 수 있다. 
2. `v-bind`와 `v-on`을 사용해서 구현하는 양방향 바인딩을 한번에 처리할 수 있다.





### 8. `v-text`, `v-html`

https://kr.vuejs.org/v2/guide/syntax.html#%EC%9B%90%EC%8B%9C-HTML

1. `v-text`는 우리가 사용하는 가장 기본 텍스트 보간법인 `{{ }}`과 같다.
2. `v-html`은 원시 HTML을 DOM에 렌더링 한다. 
   - 신뢰하지 않는 외부의 입력을 받는 경우 '절대' 사용하면 안된다.



### 9. `v-show`

https://kr.vuejs.org/v2/guide/conditional.html

1. `display: none;` 의 스타일을 적용하여 렌더링은 되지만 가시적으로 볼 수 없게 만든다. 

2. `v-if` 디렉티브와 렌더링 & 토글 비용을 비교해서 적절한 순간에 활용할 수 있어야 한다.

   - 렌더링이 자주 되지 않는 경우는 `v-if`가 유리하고 자주 토글되는 환경에서는 `v-show`가 유리하다.

   | 디렉티브 | 렌더링 비용 | 토글 비용 |
   | -------- | ----------- | --------- |
   | v-if     | 낮다.       | 높다.     |
   | v-show   | 높다.       | 낮다.     |

   

### 10.  `filters`

https://kr.vuejs.org/v2/guide/filters.html

1. 텍스트의 형식화를 적용할 수 있는 필터를 지원한다. 
2. 중괄호 보간법 혹은 `v-bind`에서 사용할 수 있다.
3. `|` 심볼을 사용하고 이전의 데이터가 필터링 로직으로 작성된 함수의 인자로 넘어온다.
4. 여러 개의 필터 요소를 chaining하여 사용할 수 있다.

5. Django의 Template filter와 유사하다.





## lodash

1. 상대적으로 Pure JavaScript만으로 순수한 프로그래밍 언어의 로직을 작성하기 어려운 경우가 있다.

   - 또한, 자주 사용하지만 구현되어 있지 않은 API가 있다.

   ```javascript
   console.log('-----------------1. reverse---------------')
   //1. reverse - Vanilla O
   // Vanilla
   const array1 = [1, 2, 3, 4]
   const reversedArray1 = array1.reverse()
   console.log(reversedArray1)
   
   // Lodash
   const array2 = [1, 2, 3, 4]
   const reversedArray2 = _.reverse(array2)
   console.log(reversedArray2) // [4, 3, 2, 1]
   
   
   console.log('-----------------2. sort---------------')
   //2. sort - Weird Operation in Vanilla 
   // Vanilla 
   const numbers1 = [10, 1, 3, 7, 4]
   // numbers1.sort()
   // console.log(numbers1)
   
   numbers1.sort(function (num1, num2) {
     return num1 - num2
   })
   console.log(numbers1)
   
   // Lodash
   const numbers2 = [10, 1, 3, 7, 4]
   const sortedNumbers2 = _.sortBy(numbers2)
   console.log(sortedNumbers2)
   
   
   console.log('-----------------3-1. range---------------')
   //3. range - Vanilla X
   // Lodash
   const nums1 = _.range(4)
   const nums2 = _.range(1, 5)
   const nums3 = _.range(1, 7, 2)
   
   console.log(nums1) // [0, 1, 2, 3]
   console.log(nums2) // [1, 2, 3, 4]
   console.log(nums3) // [1, 3, 5]
   
   console.log('-----------------3-2. random---------------')
   //3-2. random - Vanilla ?
   const randomNum1 = _.random(0, 5)
   const randomNum2 = _.random(5)
   const randomNum3 = _.random(1.2, 5.2)
   
   console.log(randomNum1)
   console.log(randomNum2)
   console.log(randomNum3)
   
   
   console.log('-----------------3-3. sampleSize---------------')
   //3-3. sampleSize - Vanilla ?
   const result = _.sampleSize([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 6)
   console.log(result)
   
   // 정렬까지
   const sortedResult = _.sortBy(result)
   console.log(sortedResult)
   ```

2. 이러한 것을 쉽게 사용할 수 있도록 해당 기능을 미리 구현 해놓은 라이브러리가 lodash다.

   https://lodash.com/

   - CDN을 활용해 사용해보자
   - `<script src="CDN을 붙여넣으세요."></script>` 

   - src 속성에 CDN 주소를 넣어서 사용하면 된다.
   - 공식 문서를 보면서 어떠한 API가 존재하는지 확인해보고 한번씩 사용해보자.

3. 최종적으로 점심 메뉴 추천과 로또 번호 추천 로직을 작성해보자
   
   - 코드는 gitlab 원격 저장소를 확인해주세요.