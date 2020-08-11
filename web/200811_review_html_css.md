### HTML

* 웹 페이지를 작성하기 위한 언어, 웹 컨텐츠의 의미와 구조를 정의

*  HTML의 기본구조

  * ```html
    <!DOCTYPE html> #html 문서 정의
    <html>
        <head> #해당 html 문서의 정보를 담고 있다. 
               #제목, 문자의 인코딩, 외부 로딩 파일 지정 
            	#브라우저에는 나타나지 않음
        </head>
        <body> #브라우저 화면에 실질적으로 나타나는 정보
            
        </body>
        
    </html>
    
    ```

  * DOM tree: 부모, 형제 관계를 보여줌

  

  * 요소(element): 태그와 컨텐츠로 구성
    * 태그 별로 속성이 있는데, 각 태그마다 사용하는 속성은 다르다.
    * 시맨틱 태그: 의미론적 요소를 담은 태그
      * 개발자 및 사용자 뿐만 아니라 검색엔진 등에 의미있는 정보의 그룹으로 표현이 가능하다.
      * 그룹 컨텐츠
        * p: 문단을 구분 지을 떼
        * hr: 문서에 가로줄 그어서 위와 아래 그룹 구분할 때
        * ol, ul, li: 리스트로 구성할 때
        * pre
        * blockquote: 인용문구 넣기
        * div: 그룹 구분지을 때
      * 텍스트 관련 태그
        * a, b, i, strong, em, span(어떠한 블록 사이에 내용 추가하고 싶을 때), br, img
      * 테이블관련태그
        * tr, td, th, thead, tbody, tfoot, caption
      * form: 서버에 처리될 데이터를 제공하는 역할
        * input: 다양한 타입을 가지는 입력 데이터 필드
        * 공통속성: name, placeholder, required, autofocus
        * type: text, radio, checkbox, date, password, ...
        * label 태그: 서식 입력 요소의 캡션 -> 어떠한 input인지 캡션을 달 수 있어서 input과 같이 사용 많이 한다. 

### 

--------

## CSS

* CSS: 스타일, 레이아웃 등을 통해서 문서를 표시하는 방법을 지정하는 언어

* CSS 적용방법

  1. inline: 태그안에 직접적으로 넣어서 사용 - 관리하기가 힘들다. 테스트 용도로 많이 씀.

     `<div style="background-color: red"></div>`

  2. 내부 참조 방식: 하나의 html 파일에서만 적용, for study or test

     `<style> h1 { color: red; } </style>`

  3. 외부 참조 방식: CSS정의를 파일 단위로 묶어서 필요한 html 문서마다 적용이 가능하다. 유지보수가 쉽다. 파일을 따로 만들어서 관리를 해야하기 때문에 CSS파일을 잘 챙겨야한다. 



* CSS 정의하는 방법

  ```
  선택자 {
  	속성: 속성값;
  }
  ```

  

* 선택자: 특정한 요소를 선택하여서 스타일링을 하기 위해 반드시 필요함.

  - 기초 선택자

    - 타입(요소, 태그) 선택자, 아이디 선택자, 클래스 선택자, 전체 선택자

  - 고급 선택자

    - 자손 선택자: 하위의 모든 요소 (`띄워쓰기`로 구분)

      선택자1 선택자2 { 속성: 속성값 }  `article p { color: red; }`

    - 직계 자손 선택자 / 자식 선택자: 바로 아래의 요소 (> 로 구분)

      선택자1 > 선택자2 { 속성: 속성값 } `div > p {color: blue; }`

    - 형제 요소 선택자: 같은 레벨에 있는 요소 ( ~로 구분)

      선택자1 ~ 선택자2 { 속성: 속성값 } `p ~ section { color: yellow; }`

    - 인접 형제 요소 선택자: 바로 붙어 있는 형제 요소 (+ 로 구분)

      선택자1 + 선택자2 { 속성: 속성값 } `div + p { color: purple; }`





### CSS 적용 우선 순위

1. 중요도(!important로 나타냄 - 무조건 0 순위, 디버깅이 어려울 수 있기 때문에 사용시 주의해야함.)

2. 우선 순위

   1. inline방식: 태그에 직접 스타일을 적용한 것

   2. 아이디 선택자

   3. 클래스 선택자

   4. ~~속성 선택자~~

      * 셀렉터[속성]: 해당 속성을 가진 요소를 선택
      * 셀렉터[속성=속성값]: 해당 속성값을 가진 요소를 선택

   5. ~~수도 클래스 선택자~~

      * 셀렉터:hover : 해당 셀렉터 위에 마우스를 오버했을 때

   6. 요소(타입, 태그) 선택자

      > 범용(*) 선택자, (' ', +, ~, >)결합자는 우선 순위에 영향을 미치지 않음.

3. 코드에 정의된 순서



* CSS 단위
  * px, %(기준사이즈에서 배율), em(상속받은 사이즈에서 배수), rem(root사이즈에서 배수)
  * vw(뷰포트 너비의 1%), vh(뷰포트 높이의 1%), vmin(뷰포트에서 가로세로 중에서 짧은 쪽의 1%), vmax(가로세로 중에 가장 긴쪽에서 1%)
  * 색상표현단위: Hex(#)-헥사데시말, rgb, hsl, rgba, hsla (a는 투명도)



* Box Model
  * margin: 바깥 여백
  * border: 테두리
  * padding: 안쪽 여백
  * content: 글이나 이미지 등의 실제 요소를 의미한다.
  * box--sizing
    * content-box: default값, 콘텐츠 영역의 크기
    * border-box: 박스 모델 테두리 기준으로 크기 조절



* 마진 상쇄
  * 수직 간의 형제 요소에서 주로 발생
  * 해결책: margin 대신 padding을 이용 (바깥여백 주지말고 아예 안쪽부터 여백 줘서 마진상쇄가 일어나지 않도록)



* CSS Display
  * block - 하나의 블록으로, 한 줄 영역을 다 차지한다.
    * div, ul, ol, li, p, hr, form
  * inline
    * span, a, img, input, label, b, em, i, strong
    * 컨텐트의 너비 만큼 공간을 차지한다. 
    * width, height, margin-top, margin-bottom은 지정할 수 없다.
  * inline-block
    * 컨텐트 너비 만큼 공간을 차지한다. 
    *  width, height, margin-top, margin-bottom을 지정할 수 있다.
  * none: 공간을 없애버림.
  * visibility: hidden - 공간은 차지하나 화면에 표시만 안함.



* CSS Position

  * static : 기본적인 배치 순서에 따름(기본값)

  * relative: static 위치= 자기자신의 초반 위치를 기준으로 이동한다.

    > 원래 공간을 남겨두고 이동하는 느낌

  * absolute: static 속성이 아닌, 가장 가까운 부모 혹은 조상 요소를 기준으로 이동

    > 기본적인 배치 순서에서 제외됨. = 원래 공간을 없애고 이동하는 느낌 

  * fixed: 부모 요소와 관계 없이 브라우저를 기준으로 위치한다.

  * sticky: relative처럼 기본 배치 순서는 가지고 있지만, 화면 밖으로 벗어나면 fixed처럼 위치에 고정되어 있다.  = 부모 영역이 존재하는 한 fixed처럼 있다.

    > 부모의 영역이 화면 밖으로 벗어나면, 다시 일반적인 흐름을 따라서 사라진다. 



vs code - python 아닌 곳에서 tab누르면 스페이스2개 띄우도록 하는 거

f1누르고 open settings json 하단에 이거 추가

기존에 있던 거 맨 뒤에 , 찍고 추가하기

딕셔너리 형태라서

```json
  "editor.tabSize": 2,
  "[python]": {
    "editor.insertSpaces": true,
    "editor.tabSize": 4
  }
```

