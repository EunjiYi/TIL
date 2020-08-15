### Float

* 한 요소(element)가 정상 흐름(normal flow)으로 부터 빠져나와, 텍스트 및 인라인 요소가 그 주위를 감싸는 형태. 자기 컨테이너의 좌, 우측을 따라 배치됨
* clear
  * 선행 floating  요소가 그 아래로 내려가야 하는지(normal flow로 돌아가는지)를 지정할 때 사용되는 속성
* flex box 및 그리드 레이아웃과 같은 기술이 나오기 전에 float은 열 레이아웃을 만드는데 사용되었다.
* mdm에서는 더 새롭고 나은 레이아웃기술(flex, grid) 나와서 현재 레거시한 레이아웃 기술로 분류된다.
* 결국은 텍스트 블록 내에서 float 이미지를 위한 역할로 돌아감.
* 그래도 네이버에서는 여전히 사용되고 있음을 확인할 수 있음.



### Flex (Flexible box module)

* flex 인터페이스 내의 아이템 간 공간배분과 정렬기능을 제공하기 위한 1차원 레이아웃 모델

* 축

  * justify: main 축(default: 좌 -> 우)
  * align: cross 축
  * content: 여러 줄을설정
  * items: 한 줄 설정
  * self: 개별요소 설정

* display: flex

  * 정렬하려는 부모 요소에 선언
  * inline-flex

* flex-direction: 쌓이는 방향을 설정

  * row(기본값): 좌 -> 우
  * row-reverse: 우 -> 좌
  * column: 위 -> 아래
  * column-reverse: 아래 -> 위

* justify-content: 메인 방향 축 요소들을 어떻게 배치할지 설정

  * flex-start(기본값): (좌 상단부터 시작하므로) 좌측 상단부터 차례로 배치됨.
  * flex-end: 끝나는 점부터 배치(아이템의 순서는 그대로, 정렬만 우측에서부터 시작)
  * center: 메인축 정중앙
  * space-between: 좌우정렬(양 끝 아이템은 양끝에 배치하고 균등하게 아이템을 위치시킴)
  * space-around: 아이템 좌우에 동일한 공간을 부여
  * space-evenly: 균등정렬

* align-items: cross 축 요소를 어떻게 배치할지 결정

  * 기본값: strech - 공간을 아이템으로 가득 채움.
  * flex-start, flex-end, center 
  * baseline: 아이템 내부 텍스트의 밑을 맞추는 것. 텍스트가 베이스라인기준으로 맞춰진다.

* align-self: cross 축 각 개별 아이템을 어떠게 배치할지 결정

  * 설정값은 위와 동일(기본값: auto)

* order: 정렬

  * 기본값: 0(모든 아이템)
  * 작은 숫자일수록 앞에 배치

* flex-grow: 메인 축에서 남은 공간을 각 항목에게 분배하는 방법

  * 각 아이템에 값을 줘서 그 공간만큼 할당
  * 많은 오해: `비율을 설정하는 것이 아님! 남은 공간을 균등하게 분배하는 것임!`
  * 음수는 불가능

  