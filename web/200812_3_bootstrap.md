intro

chrome 웹스토어 확장프로그램 `wappalyzer` 

해당 싸이트/프로그램이 어떤 언어를 쓰는지 알려준다.



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





#### spacing

