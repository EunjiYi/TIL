intro

트위터에서 시작함.

부트스트랩은 다양한 브라우저에서 모두 동일한 모습이 나오도롣해줌

chrome 웹스토어 확장프로그램 `wappalyzer` : 해당 싸이트/프로그램이 어떤 언어를 쓰는지 알려준다.



### 부트스트랩

download해서 확인해보자

#### bootstarp.css 

>  bootstrap-reboot.css(Forked from Normalize.css) + bootstrap-grid.css를 합친거

> .map 붙은 건 소스맵파일이라고 해서 일단은 무시하면 됨.

> `<head>`에 link로 불러온다. `hrep="bootstarp.css"`

* fork: 남의 레파지토리를 자기 레파지토리로 가져오는 것.



#### bootstrap_bundle.js이랑 bootstrap.js 중에서 bundle이 더 많은 내용을 가지고 있다.

> html `</body>` 직전에 불러옴

> `<script src="bootstrap_bundle.js>"`



부트스트랩이 크롬이라는 기본 css를 다 리셋시키고 부트스트랩꺼를 적용시킨다. 

부트스트랩을 쓰는 목적 중 하나가 `크로싱브라우저`

> 크로싱브라우저를 하기 위해서는 모두가 같은 스타일을 가져야한다. = 부트스트랩이 그걸 맞춰준다.



### Reset.css `vs` Normalize.css

-> 공통점: 둘 다 css 초기화 방법

* reset: 공격적인 solution
  * 브라우저의 자체의 스타일을 그냥 다 없애버림
  * 나중에 디버깅이 어려워진다.
* normalize: 젠틀한 solution
  * 웹표준에 맞춰서 브라우징스타일을 조정한다.
  * IE가 웹표준을 잘 안지킨다. 이걸 normalize하기 위해서
    * RESET이었으면 그냥 다 제로로 만들어버림
    * Normalize는 나머지 다른 브라우저들을 다 IE에 맞춘다.
  * bootstrap_reboot.css는 Normalize.css를 부트스트랩 자체로 커스터마이징 한 것.
  * 최근에 선호하는 방식



### CDN (Content Delivery(Distribution) Network)

> 컨텐츠(CSS, JS, Image, Text등)을 효율적으로 전달하기 위해 여러 노드가 가진 네트워크에 데이터를 제공하는 시스템
>
> 개별 end-user의 가까운 서버를 통해 빠르게 전달 가능(지리적 이점)
>
> 외부 서버를 활용함으로써 본인 서버의 부하가 적어짐.





#### spacing 종합

m: margin

p: padding

t: top

b: bottom

l: left

r: right

x: left, right

y: top, bottom

0: 0rem 0px

1: 0.25rem 4px

2: 0.5rem 8px

3: 1rem 16px

4: 1.5rem 24px

5: 3rem 48px



.mt-1 (상) = margin top

= 0.25rem = 16 * 0.25 = 4px

.my-1 (상하)

.mx-0 (좌우)

.m-4 네 방향 다 4

.mx-auto 가운데정렬

.py-0 패딩/상하/0으로 주겠다.

.bg-danger

.d-inline (display:inline)

.d-block

.fixed-bottom

.fixed-top

.d-flex 

justify-content-start (flex-start 가 아니고 그냥 start 형태)

.sticky-top : 화면에 고정되는 것은 맞는데 다음 sticky 요소를 만나면 그걸로 대체되고 밀려올라감



#### breakpoints

sm, md, lg, xl 화면이 언제언제 바뀔지 내부적으로 정해놓음





### Responsive Web Design 

* one source , multi use
* 각각의 디바이스별로 코드를 짜지 말고, 우리는 한 번만 짜고 이 하나를 멀티유즈하자.
* 하나의 웹싸이트를 구축해서 다양한 화면 해상도에 최적화를 하겠다.
* 반응형 웹은 별도의 기술이름이 아니고, 휍디자인에 대한 접근방식이다. = 반응형 레이아웃 작성에 도움이 되는 사례들의 모음 등을 기술하는데 사용되는 용어
* 예시: Media Queries, Flexbox, Bootstrap Grid System, The viewport meta tag



### bootstrap grid system은 내부적으로 flexbox 로 구성된다. 

container, row, column 으로 컨텐츠를 배치하고 정렬한다.

```html
<div class="container">
   <div class="row">    <!--row를 선언하면,  display: flex 가 자동으로 선언된다.-->
       <div class="col"></div>
       <div class="col"></div>
       <div class="col"></div>
    </div> 
</div>
```

12개 컬럼 = 하나의 웹페이지를 12개로 나눠서 사용하겠다. = 약수가 많다. 

5개의 breakpoints = 5 default responsive tiers

>이 정도는 외워야함.
>
>`.col-`   <576px
>
>`.col-sm-`   >=576px
>
>`.col-md-`  >= 768px
>
>`.col-lg-`  >=992px
>
>`.col-xl-`  >=1200px