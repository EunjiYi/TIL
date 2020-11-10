구글링 google developer console

https://console.developers.google.com/apis/dashboard?project=vue-youtube-ggume&hl=ko



새 프로젝트 만들기 (프로젝트명 `vue-youtube-ggume`)

새로 만든 프로젝트 선택 

검색창에 `youtube data api v3` 검색

`사용` 클릭



왼쪽 메뉴바 `사용자 인증 정보 만들기` 클릭

 `youtube data api v3`

웹브라우저(자바스크립트)

공개데이터 클릭



-----

구글링 youtube data api

https://developers.google.com/youtube/v3/getting-started?hl=ko

상단 바 `참조` 클릭



https://developers.google.com/youtube/v3/docs/search/list?hl=ko

### HTTP 요청

```
GET https://www.googleapis.com/youtube/v3/search
```



매개변수

part 의 속성은 `snippet` 으로.

`**q**` 매개변수는 검색할 검색어를 지정합니다. 



