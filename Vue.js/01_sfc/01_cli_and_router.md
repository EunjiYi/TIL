# 01_Vue CLI & Vue Router

> 2020.11.11 오전 라이브

[강의 코드 -  기본 프로젝트 연습](https://lab.ssafy.com/ssafy4/vue/tree/master/01_sfc/my-first-vue-cli-project)

[강의 코드 - Lunch & Lotto](https://lab.ssafy.com/ssafy4/vue/tree/master/01_sfc/lunch-lotto-with-vue-cli)



아래의 내용과 라이브 시간에 활용한 코드를 같이 살펴보며 학습하면 됩니다. 😊



## 1. Vue CLI Project

https://cli.vuejs.org/guide/

**`$ vue create my-project` 명령어를 활용해 생성되는 프로젝트의 구조를 간단하게 살펴보자**



- `node_modules`

  - `npm`으로  node.js 환경의 여러 의존성 모듈
  - `-g` 옵션을 준 경우는 전역 영역에 설치되고 해당 옵션을 빼면 프로젝트 폴더 내부(local)에 생성된다.
  - **절대로 git으로 관리하면 안되는 폴더**

- `public/index.html`

  - **The file `public/index.html` is a template that will be processed with html-webpack-plugin.**
  - Vue 앱의 뼈대가 되는 html 파일
  - `main.js`에서 `$mount('#app')` 마운트의 대상이 되는 DOM Element가 존재한다.
  - 실제 배포를 하기 우해 `npm run build`한 결과물은 이 html 문서 한 장에 모두 묶이게 된다.

- `package.json`

  https://docs.npmjs.com/cli/v6/configuring-npm/package-json

  - 일반적으로 `node_modules` 를 공유하지 않기 때문에 `package.json`의 내용을 현재의 개발 환경에서 그대로 활용하고 싶은 경우 아래와 같이 npm을 활용하면 된다.

    ```bash
    $ npm install (혹은 i)
    ```

  - 대표적인 것

    - `scripts`

      https://docs.npmjs.com/cli/v6/configuring-npm/package-json#scripts

      - 사용 할 명령어 script

    - `dependencies`

      https://docs.npmjs.com/cli/v6/configuring-npm/package-json#dependencies

      - Dev + Production Level에서까지 활용 할 모듈
      - 버전을 같이 명시

    - `devDependencies`

      https://docs.npmjs.com/cli/v6/configuring-npm/package-lock-json

      - Development Level에서만 활용 할 모듈

- `package-lock.json`

  https://docs.npmjs.com/cli/v6/configuring-npm/package-lock-json

  - Describe a single representation of a dependency tree such that teammates, deployments, and continuous integration are guaranteed to **install exactly the same dependencies.**
  - `node_modules` 에 설치되는 모듈과 관련해서 모든 의존성을 알아서 설정한다. →  쉽게 말하면 사용 할 패키지의 버전을 고정하는 것이다.
    - `npm install` 명령어에 생성된다. → 즉, 그 순간에 최신 정보를 반영한 의존성 모듈을 점검하는 것.
    - 이렇게 짧은 순간에 발생할 수 있는 협업 개발 과정 간의 의존성 패키지 충돌 등을 피할 수 있다.

- `babel.config.js`

  - 바벨 설정과 관련된 내용이 들어가는 파일

- `src/`

  - `assets/`
    - webpack에 의해 빌드된 정적 파일
  - `components/`
    - `HelloWorld.vue`
      - 하위 컴포넌트
      - 하지만 반드시 하위 컴포넌트를 넣어야 하는 것은 아니다. 
  - `App.vue`
    - 최상위 컴포넌트
  - `main.js`
    - Webpack이 빌드를 시작할 때 가장 먼저 불러오는 진입점(Entry Point)
    - 실제 단일 파일에서 DOM과 Data를 연결 했던 것과 동일한 작업이 이루어지는 파일
      - DOM에 mount하는 작업
    - Vue 전역에서 활용 할 모듈을 등록할 수 있는 파일



## 2. Vue Router

https://router.vuejs.org/kr/



### 2.1. Vue Router - 기본 개념 및 구조

**기본 환경 설정**

Vue CLI 환경에서는 매우 편하게 아래의 명령어 한 줄만 입력하면 설치할 수 있다.

- 중간에 commit 여부를 물어보는 것은 선택적으로 진행하면 되고 History 모드를 물어보는 것은 Y를 입력한다.
- router를 설정하면 프로젝트의 기본 구조가 바뀌기 때문에 사전에 commit을 통해 이전 버전으로 돌아갈 수 있도록 유도하는 것

```bash
$ vue add router
```



**`History mode` vs `Hash(#) mode`**

- router 설치 시에 물어보았던 History Mode에 대해서 간단하게 알아보자
- 아래의 내용은 참고하는 정도로만 알아두자

1. SPA (Single Page Application)

   - 단일 페이지 어플리케이션이라는 것은 말 그대로 응답 받은 하나의 html 파일에서 이동하지 않고 JavaScript를 활용해 내용만 변경하는 것이다. 
   - 이때 주소가 바뀌는 것처럼 보이는 것은 실제 주소가 변경되는 것이 아니라 내용(컴포넌트)만 변경되고 있는 것이다. 
     - 그럼 주소를 바꾸지 않으면 안되는건가? 사용자의 입장에서 생각해보면 주소가 이동하지 않는 것은 상당한 어색할 수 있다. 
     - HTML 내용은 변화하지만 계속 같은 페이지에 머무는 것 같은 느낌이 들기 때문에 사용자 경험 저하 될 수 있다.
   - 이러한 이유 때문에 URI의 변화를 통해 마치 바뀌는 화면에 맞춰 페이지가 변화되는 것처럼 느끼게 만드는 것이다.

   - 이때 사용하는 방식이 크게 Hash와 History 2가지다.

2. Hash & History 모드

   - Hash(`#`) 모드
     - 특정한 주소를 입력하고 Enter를 치는 행위는 Server에게 요청(GET)을 보내는 행위다.
     - 주소가 변경되는 건 새로운 endpoint로 서버에 요청을 보내는 것이고 그에 맞는 HTML을 받아야 한다. 하지만 서버에서 URL을 만들어 놓지 않으면 아무런 의미가 없다. 즉, Server가 URL을 만드는 주체가 된다.
     - Hash 모드는 프레그먼트(`#`) 뒤에 오는 문자열은 경로로 인식하지 않는 특징을 활용해 마치 URL이 변화하지만 실제 서버로부터의 요청을 받지 않는 형태로 구현한 것이다.
     - 하지만 `#`이 주소 안에 포함된 건 사용자에게 상당히 큰 어색함을 줄 수 있어 최근에는 잘 사용하지 않는다.
   - History 모드
     - HTML5의 스펙 중 하나인  history API를 사용해서 router를 구현한 것이다.
     - `History` API의 `pushState`라는 메소드를 사용하는데, 이 API는 브라우저의 히스토리는 남기지만 실제 페이지는 이동하지 않는 기능을 지원한다.



**프로젝트 구조 및 주요 파일**

- 기존 Vue CLI로 구성한 환경과 조금 달라진 구성을 보인다.

- 주요 변경 사항 위주로 살펴보자

```javascript
// index.js

//1. 등록 할 컴포넌트를 불러오는 구문 
import Home from '../views/Home.vue'
import TheLunch from '../views/TheLunch.vue'
import TheLotto from '../views/TheLotto.vue'

Vue.use(VueRouter)

//2. 실제 route(경로)를 작성하는 배열
const routes = [
  ...
  //3. 경로에 대한 정보를 작성
  {
    //3-1. 특정 컴포넌트와 매핑된 경로
    path: '/lunch',
    //3-2. 별명으로 부를 때 사용 할 이름
    name: 'TheLunch',
    //3-3. path와 매핑된 컴포넌트. 경로에 맞는 화면이 보여질 때 나타나는 대상
    component: TheLunch,
  },
]
```

```html
<!-- App.vue -->

<template>
  <div id="app">
    <div id="nav">
      <!-- 1. use router-link component for navigation. (어떠한 컴포넌트를 보여줄 지 결정하는 route) -->
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
       
      <!-- 경로로 부르는 방식 / 별명으로 부르는 방식 -->  
      <!-- <router-link to="/lunch">TheLunch</router-link> | -->
      <router-link :to="{ name: 'TheLunch' }">TheLunch</router-link> |
        
      <!-- 경로로 부르는 방식 / 별명으로 부르는 방식 -->  
      <!-- <router-link to="/lotto">TheLotto</router-link>  -->
      <router-link :to="{ name: 'TheLotto' }">TheLotto</router-link> 
    </div>
    <!-- 2. route outlet (실제 컴포넌트가 렌더링되어 보여지는 곳) -->
    <router-view/>
  </div>
</template>
```



- **router-link**

  https://router.vuejs.org/kr/api/#router-link

  - 이 링크는 `index.js` 파일에 정의한 경로에 등록한 특정한 컴포넌트와 매핑된다.

  - HTML5 히스토리 모드에서, `router-link`는 클릭 이벤트를 차단하여 브라우저가 페이지를 다시 로드하지 않도록 한다.
  - a 태그지만 우리가 알고 있는 GET 요청을 보내는 a 태그와 조금 다르게 기본 GET 요청을 날리는 이벤트를 제거한 형태로 구성

- **router-view**

  - route와 매칭되는 컴포넌트가 렌더링 되는 자리

  - 실제 component가 DOM에 부착되어 보이는 자리를 의미한다. 이 자리는 `router-link`와 연결되어 있다.

- 실제 Network 탭을 열고 링크를 누르고 이동 할 때 페이지의 요청이 들어오지 않는 것을 확인해보자
  -  SPA 방식으로 개발하고 있음을 보여주는 명확한 증거다. 
  - 좋아요, 팔로우 코드를 작성 했을 때와 마찬가지로 받은 문서 한장에서 처리한다.
  - Dev Tools를 통해 특정 router와 연결된 컴포넌트를 확인해보자





**프로젝트 구조화?**

- 프로젝트를 진행할 때 어떻게 폴더 및 파일을 배치 할지 여부는 개발자 본인이 직접 결정할 수 있다.

- 일반적으로는 아래와 같은 형태로 구성하여 사용한다.

  

`App.vue`

- 최상위 컴포넌트

`views/`

- router(`index.js`)에 매핑되는 컴포넌트를 모아두는 폴더

- App 컴포넌트 내부에 About & Home 컴포넌트 등록

`components/`

- router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더(자식 컴포넌트)
- Home 컴포넌트 내부에 HelloWorld 컴포넌트 등록





### 2.2. Vue Router - 추가 사항

1. 이름으로 부르는 route - [공식문서](https://router.vuejs.org/kr/guide/essentials/named-routes.html)

   - Django에서 `urls.py`의 `path` 함수의 3번째 기본값 인자였던 `name`을 사용하는 방식과 유사하다.
   - `index.js` 에 정의한 내용 중 `name`이라는 속성에 설정한 값을 주소에 활용할 수 있다. 이때 `v-bind` 디렉티브를 통해 바인딩을 해야한다.

   ```javascript
   // index.js
   
   ...
   
   const routes = [
     {
       path: '/lunch',
       name: 'TheLunch',
       component: TheLunch
     },
     {
       path: '/lotto',
       name: 'TheLotto',
       component: TheLotto
     },
   ]
   ```

   ```html
   <!-- App.vue -->
   
   <template>
     <div id="app">
       <div id="nav">
         <router-link :to="{ name: 'TheLunch' }">Lunch</router-link> |
         <router-link :to="{ name: 'TheLotto' }">Lotto</router-link> 
       </div>
       <router-view/>
     </div>
   </template>
   ```

   

2. (특정 작업 이후에) 다른 컴포넌트로 보내기 - [공식문서](https://router.vuejs.org/kr/guide/essentials/navigation.html)

   - Django의 redirect처럼(같진 않다.) 특정 작업 이후에 다른 컴포넌트를 보여줄 수 있다.
   - Vue 인스턴스 내부에서 라우터 인스턴스에 `$router`로 액세스 할 수 있다. 그러므로`this.$router.push`를 사용 할 수 있다.

   ```javascript
   // TheLunch.vue
   
   methods: {
   	pickOneInLunchMenu: function () {
   	 ...
   	  this.selectedLunchMenu = this.lunch[randomIndex]
   	  // 프로그래밍 방식의 네비게이션
   	  // this.$router.push('/lotto') -> path로 갈 수도 있고
   	  this.$router.push({ name: 'TheLotto' }) -> name 속성에 설정한 이름으로 갈 수도 있다.
   	},
   ```

   

3. 주소 창에서 넘긴 데이터를 활용하기 - [공식문서](https://router.vuejs.org/kr/guide/essentials/dynamic-matching.html)

   - Django의 Variable Routing과 유사하다.
   - 이때 주소는 `:`으로 시작하여 동적으로 넘어가는 데이터가 담길 변수의 이름을 적어주면 된다.
   - 이제 `/lotto`뒤에 넘기는 데이터는 `lottoNum`이라는 변수에 담긴다.

   ```javascript
   // index.js
   
   const routes = [
     ...
     {
       path: '/lotto/:lottoNum',
       name: 'TheLotto',
       component: TheLotto
     },
   ]
   ```

   ​	

   만약 주소 창에 `lotto/5` 로 입력하면 5개의 추첨 번호를 보여준다.

   - 그 값은 해당 컴포넌트의 `$route.params` 를 통해 접근할 수 있다.

   ```javascript
   // index.js
   
   const routes = [
     ...
     {
       path: '/lotto/:lottoNum',
       name: 'TheLotto',
       component: TheLotto
     },
   ]
   ```

   ```html
   <!-- TheLotto.vue -->
   
   <template>
     <div>
       ...
       <p>오늘의 추천 로또 번호</p>
       <!-- { "lottoNum": "5" }-->
       <!-- <h2>{{ $route.params }}</h2> -->
       <h2>로또 번호 {{ $route.params.lottoNum }}개 추천</h2>
       <ul>
         <li v-for="(num, idx) in selectedLuckyNums" :key="idx">{{ num }}</li>
       </ul>
   
       <hr>
     </div>
   </template>
   ```

   ```javascript
   // TheLotto.vue
   
   methods: {
     getLuckyNums: function () {
       const numbers = _.range(1, 46)
       this.selectedLuckyNums = _.sampleSize(numbers, this.$route.params.lottoNum)
   	// this.selectedLuckyNums = _.sampleSize(numbers, 6)
   	this.selectedLuckyNums = _.sortBy(this.sampleNums)
     },
   }
   ```

   