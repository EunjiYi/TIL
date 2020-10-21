

### JS Basic

### ECMA Script = Javascript

### ES6+



### 식별자(identifier)

* 변수명은 식별자라고 불린다.

* 규칙
  1. 반드시 문자, 달라(`$`) 또는 밑줄(`_`)로 시작해야한다. 숫자나 `-`로 시작할 수 없다.
  
     ```
     const hi-hello //안됨
     const hi+hello //안됨
     const hi_hello //됨
     
     const cats = [] //가능
     function getProperty () {}  // 가능
     ```
  
     
  
  2. 대소문자를 구별한다. 대소문자 유의
  
  3. 예약어는 사용할 수 없다. (`const`, `function`, `class` 등)
  
* 스타일

  * 카멜케이스 (LowerCamelCase)
    * 주로 객체, 변수, 함수를 선언할 때 로얼카멜케이스=(그냥 카멜케이스라 부름)를 사용한다.
  * 파스칼케이스 (UpperCamelCase)
    * 클래스, 생성자 등
  * 대문자 스네이크 케이스 (UPPER_CASE)
    * 상수를 정의할 때 사용. 해당 변수나 변수의 속성이 변하지 않는 것 = 상수



----------



### 변수



#### let

값을 재할당 할 수 있는 변수를 선언하는 키워드

> 선언은 한 번, 할당은 여러 번



* 한 번만 선언할 수 있으며 여러 번 할당할 수 있다.

  ```
  let x = 1 //선언은 한 번
  x = 3 // x를 한 번 선언했으니 그냥 3만 넣는다. 
  ```



* 재선언할 경우 SyntaxError 발생 - 이미 선언되어 있다고 나온다. 

  ```
  let x = 1
  let x = 3 //이거 안됨
  SyntaxError: Identifier 'x' has already been declared
  ```

  

* 블록 유효범위를 갖는다. (if, for, 함수 등의 중괄호 {} 내부 만큼의 유효범위를 가진다.)

  ```
  let x = 1  // if문 밖 x와
  
  if (x === 1) {
  	let x = 2 // if문 안의 x가 다른 유요범위를 가진다. = 즉 서로다른 x
  	console.log(x) // 2
  }
  
  console.log(x) // 1
  ```



* 크롬/IE는 개발자를 위해 let을 여러번 선언할 수 있게 풀어놓았다. 다만 브라우저 내에서만 풀어준거지 javascript 기능 자체가 되는 건 아님! (FireFox는 여러번 선언 안 됨 = 표준지키는 중)





#### const

값이 변하지 않는 상수를 선언하는 키워드

* 선언 시 반드시 초기값을 설정해주어야한다.

  ```
  const myFav // 안 됨
  Uncaught SyntaxError: Missing initializer in const declaration
  
  const myFav = 7 // 됨
  ```

  

* 선언도 한 번, 할당도 한 번

  ```
  myFav = 10 // 재선언 시 TypeError 발생
  Uncaught TypeError: Assignment to constant varible.
  ```

  ```
  const myFav = 10
  Uncaught SyntaxError: Identifier 'myFav' has already been declared
  ```

  

* let과 동일하게 블록유효범위(block scope)를 가진다.





#### var

* 선언도 여러번, 할당도 여러번 가능

  ```
  var num = 1
  var num = 2
  console.log(num) // 2
  ```

* 함수유효범위(function scope) = block scope 이 아니고, global 느낌. 

  ```
  var a = 1
  let b = 2
  
  if (a === 1) {
  	var a = 11 // 전역변수 a를 덮어쓴다.
  	let b = 22 // if 내의 지역변수
  	console.log(a) // 11
  	console.log(b) // 22
  }
  
  console.log(a) // 11
  console.log(b) // 2
  ```

  

* var는 예기치 않는 문제를 많이 발생시키는 키워드로, **절대 사용하지 않는다. **



### Hoisting

* var로 선언된 변수는 선언 이전에 참조할 수 있는 현상

  ```
  console.log(name) // 이거 에러 발생하지 않는다! 그냥 undefined 라고 뜸. 여기서 var name 선언됨. 에러가 발생하지 않기 때문에 놓칠 수가 있다.
  var name = '홍길동' // 이건 재정의된 것. 근데 var 특성상 재정의가 된다.
  ```

  선언 + undefined로 초기화 동시진행 -> undefined라고 나옴.

  

  비교

  ```
  console.log(age) // 에러 발생한다.
  // Uncaught ReferenceError: age is not defined
  
  let age = 10
  ```

  1. 선언
  2. TDZ(임시적 사각지대)
  3. let 변수 (undefinded) <= 이 undefined라는 초기화 전에 console.log(age)하면, TDZ(Temporal Dead Zone)때문에 에러 발생.
  4. 초기값





### 정리

var : 재할당, 재선언, 함수 스코프

let :  재할당, 블룩 스코프

const : 블록 스코프



-> 그래서 ES6부터 var 쓰지 않고 let, const를 표준으로.







---------------------------

### 타입과 연산자



#### 1. 타입 

원시자료형 = Primitiv

mdn 원시값

https://developer.mozilla.org/ko/docs/Glossary/Primitive

```
JavaScript에서의 원시 래퍼 객체
null과 undefined 를 제외하고, 모든 원시 값은 원시 값을 래핑한 객체를 갖습니다.

문자열 원시 값을 위한 String 객체.
숫자 원시 값을 위한 Number 객체.
빅인트 원시 값을 위한 BigInt 객체.
불리언 원시 값을 위한 Boolean 객체.
심볼 원시 값을 위한 Symbol 객체.

래퍼 객체의 valueOf() 메서드는 원시 값을 반환합니다.
```





자바스크립트가 가질 수 있는 Primitive 타입을 알아보자

#### Number

* 양수, 음수, 소수, 10^8등 지수 가능. 양/음무한대 가능. 숫자이긴한테 올바른 숫자는 아니야(= PYTHON에서 zero division error)

```
const a = 13
const b = -5
const c = 3.14
const d = 2.998e8
const e = Infinity // -Infinity도 가능
const f = NaN // Not a Number 숫자가 아니다. 
```



#### Boolean

* `true` (python 처럼 `True`가 아님)
* `false`



#### Empty Value

* 값이 존재하지않음을 표현한다.

* 큰 차이를 두지 않고 interchangeable하게 사용하도록 권장한다.

* null, undefined

  > 둘의 차이
  >
  > undefined: 값이 없을 경우 JAVASCRIPT가 할당하는 값.
  >
  > null:값이 없음을 표현하기 위해서 개발자가 인위적으로 사용하는 값

* undefined와 null은 typeof 연산자를 통해 서로 **다른 값이 반환**된다.

  ```
  typeof null // object 개발자가 의도적으로
  typeof undefined // underfined  JS가 자동으로 해줌
  ```

  



#### String

* js에서 문자열을 표현하는 방법

  ```
  const str1 = '작은 따옴표 사용'
  const str2 = "큰따옴표 사용"
  
  str1 + str2 // "작은 따옴표 사용큰따옴표 사용"
  
  const str3 = " 줄 바꿈은
  허락되지 않는다." // Uncaught SyntaxError: Invalid or unexpected token
  
  //escape sequence
  const str4 = "줄 바꿈은 \n 이렇게 해야합니다."
  
  str4
  "줄 바꿈은 
   이렇게 해야합니다."
   
  // Template literal (ES6+ 부터 사용가능)
  const str5 = `안녕하세요.`
  
  const str6 = `백틱을 사용하면 그냥 줄바꿈도
  가능해요`
  str6
  "백틱을 사용하면 그냥 줄바꿈도
  가능해요"
  
  //해당 변수의 값을 넣어서 보여준다.
  const num = 100
  const str6 = '${str1}'
  const str7 = `${num} - ${str1}`  // "100 - 작은 따옴표 사용"
  
  ```

* 리터럴

  * **값을 프로그램 안에 직접 지정한다**는 뜻.
  * 리터럴은 **값을 만드는 방법**





#### 2. 연산자

* 할당연산자

  https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Assignment_Operators

  (구글링 `mdn 할당연산자`)

  ```javascript
  let c = 0
  c += 10  // 10 - c에 10을 더한다.
  c -= 3   // 7 - c에 3을 뺀다.
  c *= 10  // 70 - c에 10을 곱한다.
  c++  // 71 - c에 1을 더한다. (증감식)
  c--  // 70 - c에 1을 뺀다. (증감식)
  ```

  

* 비교연산자

  * 문자열 비교는 영어 소문자가 대문자보다 더 큰 값 / 알파벳은 오름차순 순서로 뒤에 있는 게 더 큰 값.

  ```javascript
  3 > 2  // true
  3 < 2 // false
  
  'A' < 'B'  //true
  'z' < 'a'   //true
  '가' < '나'  //true
  ```

  

* 동등연산자

  * 비교 대상이 서로 다른 타입일 경우, 비교하기 전에 가능하다면 같은 자료형으로 형변환하여 비교한다. 값을 비교
  * 그러나 이런 형변환은 예기치 못한 결과를 야기할 수 있기 때문에 동등 연산자의 사용은 지양한다.

  * 그러니까 일치연산자를 사용하자!

  ```javascript
  const a = 1
  const b = '1'
  
  console.log(a == b)             // true
  console.log(a == Number(b))     // true - Number를 통해 숫자로 형변환
  ```

  

* 일치연산자

  * 타입과 값이 모두 같은지를 비교
  * 엄격한 비교를 하기 때문에 이걸 사용하길 권장한다. (동등연산자 말고 일치연산자 쓰자.)
  * 일치 `===` / 불일치 `!==`

  ```javascript
  const a = 1
  const b = '1'
  
  console.log(a === b) //false
  console.log(a === Number(b)) // true
  ```

  



* 논리연산자

  * boolean 타입을 연산할 수 있는 연산자

    > and, or, not 지원한다.
    >
    > and 연산은 &&연산자를 통해 연산한다. 모두 참일 경우 true를 반환한다.

    ```javascript
    true && false // false
    true && true // true
    
    1 && 0 // 0
    0 && 1 // 0
    4 && 7 // 7
    ```

    

    *주의*

    A and B  `python`

    A && B   `JS`

    

  *  or 연산은 || 연산자를 통해 연산한다. 둘 중 하나라도 참일 경우 true를 반환한다.

    ```javascript
    false || true   // true
    false || flase // false
    
    1 || 0   // 1
    0 || 1   // 1
    4 || 7   // 4
    ```

    

  * not 연산은 ! 연산자를 통해 연산하며, 단일 값에 사용하는 단항 연산자로 해당 논리값을 반대로 뒤집는다.

    ```javascript
    !true //false
    ```

    



* 삼항연산자

  * 조건식이 참이면: 앞의 값을 반환하며 
  * 조건식이 거짓이면: 뒤의 값을 반환하는 연산자.
  * 삼항 연산자의 중첩 사용은 지양하며, 일반적으로 한 줄에 표현한다.

  ```javascript
  true ? 1: 2 // 1
  false ? 1: 2 // 2
  
  const result = Math.PI > 4 ? 'Yep' : 'Nope'
  console.log(result) // Nope
  ```

  *주의*

  `A if 조건문 B`   `python`

  `조건문 ? A : B `   `JS`



-------



### if, else if, else

*주의*

`if elif else`   `python`

`if {} else if {} else`   `JS`



`if 조건: `    `python`

`if ( 조건 ) { }`   `JS`



#### Switch

```javascript
let name = '홍길동'

switch(name) {
		case 'admin': {
			console.log('관리자 모드')
			break  // break는 필수. 없으면 break만날 때가지 아래 전부 동작.
		}
		case 'manager': {
			console.log('매니저 모드')
			break
		}
		default: {
			console.log(`${name}님 환영합니다.`)
		}
		
}
```

만약 break가 없으면 자바스크립트가 break문 나올때까지 아래꺼 다 읽는다.
처음에 case걸리면 그 다음부터는 case무시하고 break 나올때까지 쭉



#### while(조건)  {

#### }

종료조건 안 적으면 무한루프 돈다 반드시 적기





### for 문

### for(초기값; 종료조건; 증감)

```javascript
for (let i = 0; i < 6; i++) {
    console.log(i)  // 0 1 2 3 4 5
}
```



### for of

> 배열에서 요소를 하나씩 순회하며 반복
>
> 매 요소는 블럭 내에서 새롭게 선언되기 때문에 반드시 변수 선언 키워드를 작성한다.

`for i in [a, b, c]`   `python`

```javascript
const numbers = [0, 1, 2, 3]
for (const number of numbers) {
	console.log(number)  // 0, 1, 2, 3
}
```



```javascript
const obj = {a : 'a', b: 'b'}
for (const o of obj){
	console.log(o)
} 
// 에러 출력
Uncaught TypeError: obj is not iterable
```



### for in 

Object(파이션에서 dictionary) 의 key를 순회하는 반복문 

Array의 경우 index를 순회한다. 배열의 값은 for문으로 돌려야한다. 배열을 for in으로 돌리면 인덱스를 순횐한다! (조심)

```javascript
const obj = {a : 'apple', b: 'banana'}
for (const o in obj){
	console.log(o) // 키 
    console.log(obj[o]) // 값
} 

/*
a
apple
b
banana
*/
```

```javascript
const fruits = ['apple', 'banana']

for (const idx in fruits) {
    console.log(idx)  // 0, 1
    console.log(fruits[idx])   // apple, banana
}

/*
0
apple
1
banana
*/
```

`python` - dict 값만 뽑아서 for

values() `(v)`

items() `(k, v)`



`mdn object values`



---------



### 함수

`def 함수명 (매개변수):`    `python`



`function 함수명 (매개변수) { }`   `JS`



#### 컨벤션

```javascript
// v부분은 띄어쓰기 
// 이 사이사이 띄어쓰기 하는 게 컨벤션이다.
function v 함수명 v () v {}
function v () v {}
```





* 함수선언식

  ```javascript
  function add (num1, num2) {
  	return num1 + num2
  }
  add(2, 7)   // 9
  ```

  

* 함수표현식

  이름이 없는 함수를 익명 함수(anonymous function)라고 한다. 익명 함수는 함수 표현식에서만 사용할 수 있고, 별도의 변수에 담지 않으면 호출이 불가하다.

  ```javascript
  const sub = function (num1, num2) {
      return num1 - num2
  }
  sub(7, 2)  // 5
  ```

  기명함수도 함수표현식이 가능하다.

  ```javascript
  const mysub = function sub (num1, num2) {
      return num1 - num2
  }
  mysub(7, 2)  // 5
  ```

  

* 기본인자(Default Arguments)

  ```javascript
  const greeting = function (name = '홍길동') {
      console.log(`안녕하세요 ${name}님!`)
  }
  ```

  

### Hoisting

```javascript
// 함수 선언식에서 hoisting 발생

add(2, 7)
function add (a, b) {
	return a + b
}
// 해도 동작한다.


// 함수 표현식일 때는 오류난다.
sub(7, 2)
const sub = function (num1, num2) {
	return num1 - num2
}
// function에서hoisting 현상이 나타나야하는데 const 라서 hoisting이 안됨. 
// Uncaught ReferenceError: Cannot access 'sub' before initialization


// 그렇다면 var라면? Uncaught TypeError: sub is not a function
sub(7, 2) // var sub이라는 이름만 선언되는 것 즉, undefined
var sub = function (num1, num2) { // sub는 현재 undefined지, function이 아니야.
	return num1 - num2
}

// 이거랑 같은 논리
// console.log(name) // var name 선언 시 undefined
// var name = '홍길동'

// 깨알 화살표함수 써보기
const sub = (num1, num2) => num1 - num2
```





#### 화살표함수

`mdn 화살표함수`

```javascript
const arrow = function (name) {
    return `hello ${name}!`
}

// 1. function 키워드를 삭제하고, 화살표로 표현
const arrow = (name) => {
    return `hello ${name}!`
}

// 2. 매개변수가 하나일 때 괄호 생략가능
const arrow = name => {
    return `hello ${name}!`
}

// 3. body에(즉 {}에) 표현식이 하나면 리턴까지 생략가능
 const arrow = name => `hello ${name}!`
 
 // 4. 표현식이 object 객체일 경우에는 () 안쪽에 객체 표현해야한다.
 const arrow = name => ({
     message: `hello ${name}!`
 })
```



즉, 이거 두개는 같다.

```javascript
const arrow = function (name) {
    return `hello ${name}!`
}

const arrow = name => `hello ${name}!`
```



연습

```javascript
const exam = function() {
    return `hello, world`
}

//1
const exam = () => {
    return `hello, world`
} 

//2 매개변수가 하나일 때 괄호 생략하는 거니까, 매개변수가 없어서 괄호 생략 불가
// 그런데 굳이 괄호를 없애고 싶다면, 
const exma = _ => {
    return `hello, world`
}

//3
const exam = () => `hello, world`
// or
const exam = _ => `hello, world`
```





### 자료구조(Array와 Object)



#### Array (python의 List)

```javascript
const numbers = [1,2,3,4]
numbers[0]
// 1
numbser[-1] // undefined => 정확한 양의 정수 인덱스(index)만 가능하다.
// Uncaught ReferenceError: numbser is not defined
numbers.length
// 4
numbers.reverse() // 원본 변화
// (4) [4, 3, 2, 1]
numbers
//(4) [4, 3, 2, 1]
```



#### Push

DOM 조작이 아니고 Javascript 자체의 기초 문법

classList에서 정의된 add = DOM조작

ECMA Script(ES)에서 만든 표준 문법 push = Javascript 자체의 기초 문법



```
numbers.push(100)
// 5 (push로 값 들어갔을 때 length값 리턴)

numbers
(5) [4, 3, 2, 1, 100]

// const numbers = [1,2,3,4]
// const인데 push 처럼 안에 내용 바뀌는 건 가능. 즉, 기존 array 자체니까 가능
// 새롭게 만든 array를 넣는 것은 const라서 불가능

```



#### pop

무조건 마지막 요소를 빼준다.

```
numbers.pop()
100

numbers.pop(1) //안에 값 넣어도 무조건 제일 마지막꺼 빠짐
100
```



#### shift

가장 앞에 값을 뺀다.

```
numbers.shift(10000)
//1

// pop이랑 마찬가지로 인덱스 상관없이 무조건 앞에 꺼 삭제
numbers.shift()
//1
```



#### unshift

가장 앞에 값을 추가한다.

```
numbers.unshift(1000)
//3
```



#### includes

값이 있는지 없는지 확인할 때 있으면 true, 없으면  false  반환

```javascript
numbers.includes(1)  //true
numbers.includes(0)   //false
```



#### indexOf

인덱스 값 리턴

배열 안에 동일한 값이 2개 이상이면, 가장 처음에 만나는 인덱스 리턴

만약에 없는 값이면 -1 리턴

```javascript
numbers.push('a', 'a')
numbers // [1, 2, 3, 4, 'a', 'a']
numbers.indexOf('a')  // 4
numbers.indexOf('b')  // -1
```



#### join

`numbers.join('||')`

배열을 문자열로 만들어 준다.

해당 문자열로 numbers에 있는 값들을 합쳐준다.

> 순서 주의
>
> 파이썬에서는 `'||'.join(List)`

기능은 똑같다. 문법만 다름.



---------------

### Object (객체/오브젝트)

python에서 dictionary = Javascript에서 Object



* 선언

  name : 'value'

  > name에 ' ' 없다. 'name'  아니니까 조심!

  ```javascript
  const me = {
      name: '홍길동', //key가 한 단어일 때
      'phone number': '01012345678', //key가 여러 단어일 때
      appleProducts: {
          ipad: '2018pro',
          iphone: '7+',
          macbook: '2019pro',
      },
  }
  ```

  

* 요소접근

  `python` `[ ] 아니면  .get( )`

  > Key를 식별자로 활용할 수 없는 경우 반드시 [ ]로 접근해야 한다.
  >
  > > key를 식별자로 활용할 수 없는경우는 어떤게 있는지 말씀해주실 수 있을까요?  - 식별자 못 쓰는 조건이랑 같음

  ```javascript
  me.속성
  me.name //홍길동
  me['name'] // 홍길동
  me['phone number'] // '01012345678'
  me.appleProducts // { ipad: '2018pro', ... }
  me.appleProducts.ipad // '2018pro'
  ```



* Object 축약 문법

  객체를 정의할 때 **key와 할당하는 변수의 이름이 같으면** 축약이 가능하다.

  ```javascript
  const name = '김싸피'
  const score = '80'
  
  const student = {
  	// name: name,
      // score: score,
      name,
      score, // 이렇게만 써도 student.name='김싸피' 가능.
  }
  
  console.log(student) // { name: '김싸피', score: '80'}
  ```

  ```javascript
  const sub = (n1, n2) => n1 - n2
  
  const student = {
      sub: sub
  }
  ```

  ```javascript
  const sub = (n1, n2) => n1 - n2
  
  const student = {
      sub: (n1, n2) => n1 - n2
  }
  ```

  

#### 함수의 Object 축약형

```javascript
let obj = {
    name: 'ssafy',
    sayHi: function () {
        console.log('hello')
    }
}; // 세미콜론 있어도 되고 없어도 됨.

obj.sayHi() // 'hello'


// ES6+
let obj2 = {
    name: 'ssafy',
    // 함수의 object 축약형- 키에 바로 괄호 붙이기
    sayHi () {
        console.log('hi!')
    }
}

obj2.sayHi()
```



----

### JSON (JavaScript Object Notation)

javaScript Object 형태를 가지는 문자열

#### object -> JSON (string)

```javascript
const objData = {
    coffee: 'Americano',
    icecream: 'Bravo corn',
}

const jsonData = JSON.stringify(objData)
console.log(typeof(jsonData)) // string

jsonData찍어보면
"{"coffee":"Americano","icecream":"Bravo corn"}"
```



#### JSON -> object 

```
const objData2 = JSON.parse(jsonData)
console.log(typeof(objData2))  // object

objData2 찍어보면
{coffee: "Americano", icecream: "Bravo corn"}
```



-------

### Array Helper Method(ES6+ Features)

> 자주 사용하는 로직을 재활용할 수 있게 만든 일종의 라이브러리



#### forEach문

`반환값이 따로 없다.`

주어진 callback 함수를 배열의 각 요소에 대해 한 번씩 실행한다. [ 0, 1, 2, 3 ]

문법(기본형태)

> `arr forEach ( 함수 (element[, index, array]  ) )`

```javascript
// exercise
// 배열 안에 있는 정보를 곱해서 그 넓이를 areas 배열에 저장하라.

const images = [
    { height: 10, width: 30 },
    { height: 10, width: 90 },
    { height: 10, width: 32 },
]

const areas = []
```

```javascript
images.forEach(function (img) {
    areas.push(img.height * img.width)  // { height : 10, width: 30 }
})

console.log(areas)
// (3) [300, 900, 320]
```





### map

배열 내 모든 요소에 대해 주어진 callback 함수를 실행하며, 함수의 반환값을 요소로 하는 `새로운 배열을 반환한다.` 배열을 다른 모습으로 바꿀 때 사용한다.

새로운 배열을 반환한다. - 이것만 forEach문과 다름.



* exercise

  ```javascript
  // 각 숫자들의 제곱근이 들어있는 새로운 배열을 만드세요.
  
  const newNumbers = [4, 9, 16]
  ```

  

```javascript
const newArray = newNumbers.map(function (num) {
	return num ** 0.5
})
console.log(root)

//다른 방법
const newArray2 = newNumbers.map(Math.sqrt)
console.log(newArray2)
```



* 위의 forEach 연습문제를 map으로 풀기

  ```javascript
  const images = [
      { height: 10, width: 30 },
      { height: 10, width: 90 },
      { height: 10, width: 32 },
  ]
  
  const areas2 = images.map(function (img) {
  	return img.height * img.width
  })
  console.log(areas2)
  ```

  



#### filter

주어진 callback 함수의 어떤 조건을 만족하는 요소만 가지고 새로운 배열로 반환

callback 함수를 통해 원하는 요소만 추릴 수 있다.

```javascript
const products = {
       { name: 'cucumber', type: 'vegetable'},
       { name: 'banana', type: 'fruit'},
       { name: 'carrot', type: 'vegetable'},
       { name: 'carrot', type: 'vegetable'},
}

const fruits = products.filter(function (product) {
    return product.type === 'fruit'
})
console.log(fruits)
```



```javascript
// exercise
// numbers 배열을 50보다 큰 값들만 모은, 배열 filteredNumbers 를 만드세요.

const numbers = [15, 25, 35, 45, 55, 65, 75, 85, 95]
```

```javascript
const filteredNumbers = numbers.filter(function (num) {
    return num > 50
})
console.log(filteredNumbers)
// (5) [55, 65, 75, 85, 95]
```





filter는 function을 만족하는 객체 자체를 가져오기 때문에 조건문을 줘도 객체 전체를 반환한다. 

```javascript
const products = [
    {name:'cucumber', type:'vegetable'},
    {name:'banana', type:'fruit'},
    {name:'carrot', type:'vegetable'},
    {name:'apple', type:'fruit'},
]
const fruits = products.filter(function (product) {
    if(product.type === 'fruit'){
        return product['name'] // 의미없다.
    }
})
console.log(fruits)

// 결과
(2) [{…}, {…}]
0: {name: "banana", type: "fruit"}
1: {name: "apple", type: "fruit"}
length: 2
__proto__: Array(0)
```

```javascript
const products = [
    { name: 'cucumber', type: 'vegetable' },
    { name: 'banana', type: 'fruit' },
    { name: 'carrot', type: 'vegetable' },
    { name: 'apple', type: 'fruit' },
]
const fruits = products.filter(function (product) {
    if (product.type === 'fruit') {
        return Object.values(product)  //의미없다.
    }
    // return product.type === 'fruit'
})
console.log(fruits)

// 결과
(2) [{…}, {…}]
0: {name: "banana", type: "fruit"}
1: {name: "apple", type: "fruit"}
length: 2
__proto__: Array(0)
```



---------------

### find

처음 만나는 하나만.

filter는 만족하는 모든 요소. (차이)



```javascript
const products = [
       { name: 'cucumber', type: 'vegetable'},
       { name: 'banana', type: 'fruit'},
       { name: 'carrot', type: 'vegetable'},
       { name: 'carrot', type: 'vegetable'},
]

const select = products.find(function (product) {
    return product.type === 'vegetable'
})

console.log(select) // {name: "cucumber", type: "vegetable"}

```





#### some

배열 안의 하나의 요소라도(`1개 이상`) callback 함수를 만족하면 true

빈 배열에서 호출하면 false

```javascript
const products = [
       { name: 'cucumber', type: 'vegetable'},
       { name: 'banana', type: 'fruit'},
       { name: 'carrot', type: 'vegetable'},
       { name: 'apple', type: 'fruit'},    
]

const someVegetable = products.some(function (product) {
    return product.type === 'vegetable'
})
console.log(someVegetable)  // true



const someWater = products.some(function (product) {
    return product.type === 'water'
})
console.log(someWater) // false


const zeroList = []
const someZero = zeroList.some(function (zero) {
    return zero > 50
})
console.log(someZero)  // false
```

```javascript
// requests 배열에서 status가 pending인 요청이 있는지 확인하세요.
const requests = [
     { url: '/photos', status: 'complete'},
     { url: '/albums', status: 'pending'},
     { url: '/users', status: 'failed'},
]


const checkPending = requests.some(function (request) {
    return request.status === 'pending'
})
console.log(checkPending) // true가 하나라도 있어서 true
```





#### every

배열 안의 모든 요소(`all`)가 callback 함수의 테스트를 만족하면 true 반환.

빈 배열에서 호출 시 true 반환.

```javascript
// users 배열에서 모두가 submited 인지 여부를 hasSubnmitted 에 저장하세요.
const users = [
    { id: 21, submmited: true },
    { id: 33, submmited: false },
    { id: 712, submmited: true},
]

const hasSubmitted = users.every(function (user) {
    return user.submitted
})
console.log(hasSubmitted) // false가 하나라도 있으면 false 리턴.
```



*주의*

some이랑 every 헷갈리지말자.

some : 한 개라도 만족시 true

every: 모든 게 만족해야 true





### reduce

배열의 각 요소에 대해 주어진 callback 함수를 실행하고, 하나의 결과값을 반환한다. 

**reduce는 배열 내에 숫자 총합, 평균 계산 등 배열의 값을 하나로 줄이는 동작을 한다. **



#### 문법이 다른 것과 조금 다르다.

`arr.reduce(callback(acc, element, index, array), initialValue)`

- callback 함수의 첫번째 매개변수 (acc) : 누적 값 전 단계의 결과
- initialValue는 반환할 누적 값의 초기값. = acc 의 초기값 ( 생략 시 첫 번째 요소가 누적값이 된다. )
- callback 함수에서 반환하는 값이 누적값이 된다. 



```javascript
const scores = [90, 90, 80, 77]

const totalScore = scores.reduce(function (sum, score) {
	return sum + score  // sum = sum + score
})
console.log(totalScore) // 337
```



```javascript
const scores = [90, 90, 80, 77]

const totalScore = scores.reduce(function (sum, score) {
	return sum + score  // sum = sum + score
}, 10000) // 이 마지막 1000은 sum의 초기값이 들어간다! 조심!
console.log(totalScore) // 10337
```



주어진 baseUrl 문자열 뒤에 필수 요청 변수인 api의 key - value 값을 key=value 의 형태로 더하여 요청 url을 만드세요. URL에서 요청 변수는 & 문자로 구분합니다.

```javascript
const baseUrl = 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?'

const api = {
  'key': 'API_KEY',
  'targetDt': '20200115'
}

const apiUrl = Object.entries(api).reduce(function (url, api) {
    return url + `${api[0]}=${api[1]}&`
}, baseUrl)
console.log(apiUrl)

// 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=API_KEY&targetDt=20200115&'
```

`**Object.entries()**` 메서드는 [`for...in`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...in)와 같은 순서로 주어진 객체 자체의 enumerable 속성 `[key, value]` 쌍의 배열을 반환합니다.

