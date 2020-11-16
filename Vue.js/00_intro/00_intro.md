# 00_Vue.js Core Things

> 2020.11.09 오전 라이브



## Hello Vue.js ?

**Front-End Development**

> "프론트 엔드 개발은 HTML, CSS, JavaScript를 활용해서 데이터를 눈으로 볼 수 있게 만들어 준다.  결과적으로 사용자는 데이터를 직접 눈으로 보며 소통하고 상호작용 할 수 있다."

- Front-End Framework 3대장
  - React
  - Angular
  - Vue.js



**CSR(Client Side Rendering)**

- Server에서 비어있는 HTML을 제공하면 Client에서 최종적인 내용물을 채워서 렌더링
- Vue.js로 구현



**SSR(Server Side Rendering)**

- Server에서 완성된 결과물을 제공하면 Client는 그 결과물을 렌더링
- Django로 구현 



**SPA(Single Page Application)**

- 단일 페이지 어플리케이션
- 서버로부터 최초로 응답 받은 HTML 문서 안에서 (거의) 모든 것을 한다.
  - 데이터를 가져오거나 가공하는 행위 나아가 동적인 조작까지 가능하다.

- HTML 문서 전체를 새로 갱신하지 않아도 되므로 부드러운 퍼포먼스를 보여준다. 
  - 이는 결과적으로 사용자 경험(UX)의 향상으로 이어진다.

- Django를 사용해서 작성한 좋아요, 팔로우 로직은 응답 받은 페이지 내에서 AJAX 요청을 통해 페이지 갱신 없이 데이터를 조작하여 DOM을 변화 시켰기 때문에 SPA를 일부 활용했다고 할 수 있다.



## Why Vue.js ?

**모던 웹의 특징**

- 한 페이지 내에 수많은 데이터가 존재한다. 
  - 페이스북, 인스타그램
- 각 데이터의 변화 상황을 추적 & 관리하기 매우 까다로운 구조

- 순수한 JavaSript를 활용한 구현은 모든 데이터를 선택 & 변경하기 어렵고 규모가 큰 서비스의 경우 매우 큰 비용이 발생한다. 



**Vue.js의 특징**

- Vue.js는 data의 변경 사항에 집중할 수 있도록 DOM은 data의 변화에 따라 알아서 re-rendering 하도록 구현

- DOM과 data가 연결되어 있기 때문에 data 자체에 집중하여 서비스를 개발할 수 있다.



## Concept of Vue.js ?

**MVVM**

- Model
  - Plain JavaScript Object
  - Vue Instance 내부에 있으면 반응형이 된다.
- View
  - DOM(HTML)
  - Vue Instance에 의해 관리된다.

- ViewModel
  - 모든 Vue Instance는 ViewModel이다.
  - Model과 View 사이의 중개자 역할을 한다. View와 Model 사이를 동기화 시킨다.



## 참고

1. CDN(Content Delivery Network)
   - 웹 콘텐츠를 제공하는 서버를 의미하는 것으로 원본 서버(origin)가 가지고 잇는 컨텐츠를 분산해서 활용할 수 할 수 있도록 한다.
   - 본 서버와 물리적인 거리 때문에 발생하는 속도 저하 문제도 방지하고 본 서버로 요청이 몰려 서버 장애가 발생하는 이슈도 막을 수 있다.
   - Bootstrap, Lodash, Vue.js 등 많은 라이브러리, 프레임워크를 CDN을 통해 사용할 수 있다.
   - CDN을 통한 개발이 가능한지 여부는 Network Tab에서 해당 서버에 대한 요청 응답 코드가 200으로 왔는지 확인하면 된다.

2. 공식문서 Quick Start

   - 많은 프레임워크, 라이브러리에서는 해당 도구를 사용하는 간단한 방법(tutorial)을 제공한다.
   - Vue.js도 Quick Start를 통해 빠르게 프레임 워크 맛보기를 해볼 수 있다. 
   - 아래의 코드는 Vue.js 공식 문서의 '시작하기' 코드를 한 파일 내에서 확인할 수 있게 가공한 것이다.

   ```html
   <!DOCTYPE html>
   <html lang="ko">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Vue Quick Start</title>
   </head>
   <body>
     <!-- 2. 선언적 렌더링 -->
     <h2>선언적 렌더링</h2>
     <div id="app">
       {{ message }}
     </div>
     <hr>
   
     <!-- 3. 엘리멘트 속성 바인딩 -->
     <h2>Element 속성 바인딩</h2>
     <div id="app-2">
       <span v-bind:title="message">
         내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
       </span>
     </div>
     <hr>
   
     <!-- 4. 조건 -->
     <h2>조건</h2>
     <div id="app-3">
       <p v-if="seen">이제 나를 볼 수 있어요</p>
     </div>
     <hr>
   
     <!-- 5. 반복 -->
     <h2>반복</h2>
     <div id="app-4">
       <ol>
         <li v-for="todo in todos">
           {{ todo.text }}
         </li>
       </ol>
     </div>
     <hr>
   
     <!-- 6. 사용자 입력 핸들링 -->
     <h2>사용자 입력 핸들링</h2>
     <div id="app-5">
       <p>{{ message }}</p>
       <button v-on:click="reverseMessage">메시지 뒤집기</button>
     </div>
     <hr>
   
     <div id="app-6">
       <p>{{ message }}</p>
       <input v-model="message">
     </div>
     <hr>
   
     <!-- 1. Vue CDN -->
     <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
     <script>
       // 2. 선언적 렌더링
       var app = new Vue({
         el: '#app',
         data: {
           message: '안녕하세요 Vue!'
         }
       })
   
       // 3. 엘리먼트 속성 바인딩
       var app2 = new Vue({
         el: '#app-2',
         data: {
           message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
         }
       })
   
       // 4. 조건
       var app3 = new Vue({
         el: '#app-3',
         data: {
           seen: true // false로 토글 가능
         }
       })
   
       //5. 반복
       var app4 = new Vue({
         el: '#app-4',
         data: {
           todos: [
             { text: 'JavaScript 배우기' },
             { text: 'Vue 배우기' },
             { text: '무언가 멋진 것을 만들기' }
           ]
         }
       })
   
       //6. 사용자 입력 핸들링
       var app5 = new Vue({
         el: '#app-5',
         data: {
           message: '안녕하세요! Vue.js!'
         },
         methods: {
           reverseMessage: function () {
             this.message = this.message.split('').reverse().join('')
           }
         }
       })
   
       var app6 = new Vue({
         el: '#app-6',
         data: {
           message: '안녕하세요 Vue!'
         }
       })
     </script>
   </body>
   </html>
   ```

3. Vetur & Chrome Dev Tools 설치

   - Vetur
     - 기본적으로 vs code를 통해 개발을 Vue.js 개발을 하는 경우 Extenstion 중 'Vetur'를 많이 사용한다. 
     - 600만건 이상의 다운로드가 이루어졌으며 단축 문법, 자동 완성 등의 편의 기능을 제공한다.

   - Chrome Vue Dev Tools
     - Vue.js 개발 환경에서 data, event, vuex와 같은 핵심 로직의 변화를 관찰하고 디버깅 할 수 있는 도구다.
     - 크롬 익스텐션으로 설치가 가능하며 파일과 시크릿 모드에서 사용하려는 경우에는 **익스텐션의 세부정보**에서 기능에 대한 버튼을 활성화 해야한다.(중요)