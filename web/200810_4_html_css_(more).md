html css

hyper text markup language

> 웹표준을 만드는 곳은 W3C
>
> 웹표준에 대한 문서를 잘 정리해둔곳: Mozilla재단



> 표를 만들때 반드시 `<th>` 태그를 쓸 필요는 없다. 
>
> `<th>`없어도 표는 만들 수 있다. 



> 제목(Heading)태그는 제목 이외에는 사용하지 않는 것이 좋다.
>
> 시맨틱적인 의미를 담기 위해서는 `h태그`는 `제목에 주로 사용하는 것을 권장`한다. (강제 ㄴㄴ 권장임)



> 리스트를 나열하기 위해서 `<ul>` `<ol>`이 있다. `<ul>`만 사용할 수 있는 건 아님



> HTML의 모든 태그가 반드시 별도의 닫는 태그가 필요한 것은 아니다.
>
> `싱글태그(닫는태그가 없는)`도 존재한다.



> `<img>`태그로 이미지 삽입할 때는 절대경로가 아닌 `상대경로로 작성`한다.
>
> github에 업로드하거나 전체폴더의 위치가 변경되었을 때 이미지를 불러 올 수 없게 되기 때문이다 .
>
> `.` 현재폴더를 기준으로 이동
>
> `..` 한 개 상위 폴더를 기준으로 이동



> 이미지에 하이퍼링크(Hyper Link)거는 법
>
> ``` 
> <a href="링크주소">
> 	<img src="이미지상대주소.png" alt="이미지안뜨면나올문구">
> </a>
> ```
>
> 쉽다 어렵게 생각하지 말자.



CSS 

Cascading Style Sheets

> HTML과 CSS는 각각 각자만의 문법을 가지는 별개의 언어다.



> 웹브라우저는 내장 기본 스타일이 있어서 CSS가 없어도 작동한다.
>
> 아무런 CSS없어도 기본적인 HTML 태그만으로도 보여줌(기본적인 내장 스타일이 적용되어 있음)



> 자식요소 프로퍼티가 부모의 모든 프로퍼티를 전부 상속받지는 않는다.  
>
> `text요소는 상속`받는데 `block 관련 요소는 상속받지 않는다.`



> 디바이스마다의 화면 크기가 다른 것을 고려해서, `뷰포트`가 담당. 
>
> 뷰포트의 크기를 기준으로 화면을 고려한다. 
>
> 그래서 뷰포트를 고려한 단위인 vw(화면 너비의 100분의 1) vh(화면 높이의 100분의 1) vmin(화면 높이, 너비 중 작은 것의 100분의 1) vmax(화면 높이, 너비 중 큰 것의 100분의 1)를 사용한다.  (상대단위)
>
> %아님



> id는 해당 HTML문서 안에서는 유일한 값이 되어야한다. 반드시 고유값이어야한다. 중복되어서는 안된다.
>



> CSS 우선순위(높은 순)
>
> !Important -> inline style -> id 선택자 -> class 선택자 -> 요소 선택자 -> 소스 순서





-----------------------------



> **의사클랙스=가상클래스** 조심하기
>
> nth-child()와 nth-of-type()
>
> 둘다  형제 사이에서의 순서에 따라 요소를 선택해, 특정 태그에만 속성을 지정할 수 있다.

> nth-child() : 모든 요소를 포함해서 그 순서에서 찾는다. 즉 `<h2>`태그와 `<p>`태그를 모두 포함해서 순서를 센다.
>
> nth-of-type(): 해당하는 자식 태그(여기서는 `<p>`태그) 요소에서만 순서를 찾는다.



```css
/* 목록의 두 번째 <li> 선택 */
li:nth-child(2) { 
  color: lime;
}

/* 임의의 그룹에서 네 번째에 위치하는 모든 요소 선택 */
:nth-child(4n) {
  color: lime;
}
/*요소의 인덱스는 1부터 시작하지만 n은 0부터 증가한다 조심!*/
```





#### Semantic Tag = 콘텐츠의 의미를 명확히 하기 위해 HTML5에서 새롭게 추가됨

```
header, section, footer
```





#### Form, input, labe tag 연습하기

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <section>
        <form action="#"><!--일단 어디로 넘겨 갈 건 없으니 #처리하기-->
            <label for="username">USERNAME : </label>
            <input type="text" id="username" placeholder="아이디를 입력 해 주세요." autofocus><br>
            <!--보통 id를 label에 보여지는 것과 동일하게(여기서는 username) 맞춘다.알아보고 기억하기 쉽게-->
            <!--placeholder는 input박스 안에 들어있는 것.-->
            <!--USERNAME 글자를 클릭하면 아이디를 입력하는 input박스에 커서가 focusing이 되는데, 그 이유는 for="username"으로, label과 input을 연결시켜줬기 때문이다.-->
            
            <label for="pwd">PWD : </label>
            <input type="password" id="pwd">
            <!--이것역시 pwd 글자를 클릭하면 비밀번호를 입력하는 input에 focusing이 된다. for="pwd"로 label과 input의 id를 연결시켰기 때문-->
            <!--보통 id를 label에 보여지는 것과 동일하게(여기서는 pwd) 맞춘다.알아보고 기억하기 쉽게-->
            <!--일반 type="button"은 그냥 눌리는 버튼 역할-->
            <!--input type="button" value="로그인"-->
            
            <!--submit은 form태그에 설정된 action으로 해당 데이터를 묶어서 전송해줌.-->
            <input type="submit" value="서브밋">
        </form>
    </section>    
</body>
</html>
```






### em  rem

크기단위 em: 상속된 사이즈나 기본 사이즈에 대해 상대적인 사이즈. 즉, 요소에 지정된 상속의 영향으로 사이즈가 의도치 않게 변경될 수 있다.. 

크기단위 rem: em의 위험(?)을 예방하기 위해 HTML 최상위 요소(root)의 사이즈를 기준으로 삼는 크기





### 절대중요

### 자손선택자와 자식선택자의 차이

```
자손선택자: 띄워쓰기로 구분하며, 하위의 모든 요소를 선택한다. (손자, 증손자 다 포함)
```

```
자식선택자: > 로 구분하며, 바로 아래의 요소를 선택한다. (딱 자식만)
```

```html
/* 자손 선택자 */
div p {
	color: red;
}
/* div와 p를 한 칸 띄웠고, div 하위의 모든 p요소(=자식, 손자, 증손자 등등 모든 자손들 다 포함)에 대해 색깔을 red로 지정한다.

/* 자식 선택자 */
div > p {
	color: red;
}
/* div > p로 >를 사용하였고, div바로 아래에 직접적으로 감싸져있는 p요소(=딱 자식만)에 대해 색깔을 red로 지정한다.
```





### CSS flex-direction

1. Flex-direction(CSS 속성값) = flex box의 주축을 변경

   - `row`: 주 축을 좌에서 우로 설정
   - `column`: 주 축을 위에서 아래로 설정
   - `row-reverse`: 주 축을 우에서 좌로 설정
   - `colunm-reverse`: 주 축을 아래에서 위로 설정

2. Flex-direction (Bootstrap 클래스 명) = flex-direction의 4가지 요소와 대응하는 bootstrap 클래스임. - `flex`붙이면 된다.

   - row: `flex-row`
   - colunm: `flex-colunm`
   - row-reverse: `flex-row-reverse`
   - column-reverse: `flex-column-reverse`

3. align-items (CSS 속성값)

   - `stretch`: 부모 속성의 높이에 맞게 최대 높이로 늘여서 맞춤
   - `center`: 수직 축의 가운데 정렬
   - `start`:  수직 축의 시작점부터 정렬
   - `end`: 수직 축의 끝점부터 정렬

   

4. `flex-flow`는 축약형이다. (두 가지 속성의 축약형임) =  `flex-direction` + `flex-wrap`

5. 코드에 bootstrap grid system에 적용시키려면 container, row 클래스를 지정해야한다.

   ```html
   <div class="container">
   	<div class="row">
   		<div class="col-어쩌구-저쩐속성"></div>
       </div>
   </div>
   ```

   

6. bootstrap grid system에서 요소의 크기를 지정하기 위해서는 클래스를 지정해야하는데, (`breakpoint prefix`)

   ```html
   <div class="col-A-B"></div>
   <!-- A: breakpoint가 들어갈 수 있음. sm md lg xl 화면의 크기에 따라 적용되는 클래스를 다르게 하기 위해 사용한다.
   B: 1~12까지 정수가 들어갈 수 있음. 12개로 나누어진 공간에서 몇 칸을 차지하는지 표시-->
   ```

   





### 기본 그리드 레이아웃

```html
<div class="container">
    <div class="row">
      <div class="col">
        <h1>들어갈문구</h1>
      </div>
    </div>
    
    <div class="row">
      <div class="item col">
        <p>4개</p>
      </div>
      <div class="item col">
        <p>4개</p>
      </div>
      <div class="item col">
        <p>4개</p>
      </div>
    </div>
    
    <div class="row">
      <div class="item col-3">
        <p>1개</p>
      </div>  
      <div class="item col-6">
        <p>1개</p>
      </div>
      <div class="item col-3">
        <p>1개</p>
      </div>
```



### 반응형그리드

```html
<div class="container">
    <div class="row">
      <div class="item col-12 col-md-4 col-xl-2">
        <p>컬럼 한 개</p>
      </div>
      <div class="item col-12 col-md-4 col-xl-2">
        <p>컬럼 한 개</p>
      </div>
      <div class="item col-12 col-md-4 col-xl-12">
        <p>컬럼 한 개</p>
      </div>
    </div>
  </div>
```

코드 설명 `<div class="item col-12 col-md-4 col-xl-2">`

​	> Viewport 너비가 768px 미만인, 경우 각 컬럼이 12칸, 12칸, 12칸씩 차지

​	> 768px 이상인 경우, 4칸, 4칸. 4칸씩 차지

​	> 1200px 이상인 경우, 2칸, 2칸씩 차지하고 12칸을 차지하는 컬럼이 그 다음에
내려오도록 클래스 추가한 것.





그리드 꼭 직접 실습하기 심화가 중요!