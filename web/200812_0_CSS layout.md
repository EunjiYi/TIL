### CSS layout

웹페이지에 포함되는 요소들을 취합하고 그것들이 `어느 위치에 놓일 것인지를 제어`하는 기술



### css page layout techniques

- Display

- Position

- Float: `플로트 주변으로 텍스트나 인라인 요소들이 플로트 요소를 감싸는 형태`

  > 웹페이지의 특정한 요소를 위로 띄우는 것.  플롯된 요소는 기존 흐름을 벗어난다.
  >
  > float: inline과 text요소가 플롯된 요소를 감싸도록.
  >
  > clear: left (왼쪽만 플롯속성을 지워라.) = 기존 흐름을 벗어난 것을 원래대로 되돌릴 수 있다.
  >
  > clear: both(왼쪽 오른쪽 다)
  >
  > > *float 속성*
  > >
  > > none: 기본값
  > >
  > > left: 요소를 왼쪽으로 띄움
  > >
  > > right: 요소를 오른쪽으로 띄움

- Flexbox

  > flexbox: 플랙스박스는 지정한 대로 가능
  >
  > grid: 그리드는 항상 배치를 할 때 오와 열이 딱딱 맞는 형태
  >
  > -> 플랙스박스로 그리드를 충분히 표현가능.

- Grid

- Table layout

- Multiple-column layout



### Flexbox

### CSS Flexble Box Layout

보통 부모요소에 설정을 하면 자식이 따라감. 자식에게만 특수로 적용하는 경우도 있기는 함.

**부모가 자식의 레이아웃을 설정하기 때문에 display:flex를 부모요소에 선언한다.**

> flexbox = 한방향으로 정렬을 강력하게 하기 위해서 생김
>
> `단방향`임 = 동시에 두 방향으로 안 움직임 / 오른쪽갔다가 위로 등
>
> 요소/ 축
>
> 우유박스에 우유를 담는게 전부



요소 간 공간 배분과 정렬 기능을 위한 1차원(단방향) 레이아웃

```
플렉스박스는 딱 4개 (요소 2개 축 2개)
4개 개념만 알고있으면, 전부야
```

* 요소

  > Flex Container (부모 요소), Flex Item (자식 요소)

* 축

  > main axis (메인축), cross axis (교차축)



**부모요소에 display: flex 설정하면: 부모는 여전히 block 요소**

**부모요소에 display: inline-flex 설정하면: 부모도 inline이 된다. **





-------------

### 메인축만 잘 따라다니자. = 계속 메인흐름만 쫓아다녀라 메인축만 봐

**justify-content** : main axis 정렬

> `flex-direction: row` 디폴트값
>
> `flex-direction: row-reverse` 도 있음

- flex-start (디폴트값):  시작 지점에서 쌓임 (왼쪽 → 오른쪽)

- flex-end: 쌓이는 방향이 반대, 아이템의 순서는 그대로, 정렬만 우측에 되는 것 

- center

- space-between: 좌우 정렬 , item 간의 간격 동일

- space-around: 균등 좌우 정렬  

  > 내부 요소끼라의 여백 = 외곽~첫번째 요소 사이 or 마지막요소~외각 사이의 여백 x 2배)

- space-evenly: 균등 정렬 (내부 요소 여백과 외각 여백 모두 동일)

~~어째 한국말이 더 어렵다~~



**align-items**: cross axis 정렬,  여러 줄 정렬

> `flex-direction: row` 디폴트값

- stretch (디폴트 값) : 컨테이너를 가득 채움, 주어진 공간에 꽉 맞게
- flex-start: 위
- flex-end: 아래
- center
- baseline: item 내부의 text에 기준선을 맞춤, text가 얹어진 눈에 보이지 않는 라인. baseline 덕분에 상자 크기가 뒤죽박죽이어도 text끼리는 한 줄에 있는 것처럼 보임.



**align-self**: align-items 와 동일 (단, 개별 item 에 적용하기 때문에 자식요소 자체에다가 선언한다.)

- auto (디폴트값)
- flex-start
- flex-end
- center
- baseline
- stretch: 부모 컨테이너에 자동으로 맞춰서 늘어난다. (Stretch 'auto'-sized items to fit the container)



<정리>

justify 붙으면 메인축

align 붙으면 교차축

content 가 여러 줄, items 가 한 줄 정렬을 위한 속성. 

개별 요소는 self를 사용

+

참고

flex-shrink: 박스를 뚤고 나가느냐

flex-basis: 박스너비의 기본값을 어떻게 잡아나가느냐







**flex-direction**

>  쌓이는 방향 설정 (main-axis 의 방향만 바뀜. flex 는 single-direction layout concept 이기 때문)

- row (기본값)
  - 가로로 요소가 쌓임
  - row 는 주축의 방향을 왼쪽에서 오른쪽으로 흐르게 한다.
- row-reverse
- column
  - 세로로 요소가 쌓임
  - column 은 주축의 방향을 위에서 아래로 흐르게 한다.
- column-reverse





**flex-wrap**

>  item들이 강제로 한 줄에 배치 되게 할 것인지 여부 설정

### wrap nowrap

얼만큼 감싸서 여러줄로 나누느냐(wrap), 아니면 줄바꿈을 해도 다 한줄로 보느냐. (nowrap)

#### nowrap

> 우리 눈에는 두 줄로 보이지만 인식은 한 줄로 되어 있음

#### wrap

> 다른 새로운 한 줄 만듬,  즉 2줄 = 몇개를 감쌀지를 설정할 수 있다.
>
> 부모요소에 wrap으로 설정(정확히 몇개인지 감쌌을 때)되어있을때만 align-content가 먹는다. 
>
> wrap이어도 한줄이면 align-items사용가능. 여러개면 align-content사용하니까 그냥 wrap일때는 거의 align-content쓴다고 보면된다.

> wrap-reverse 넘쳤는데 위로 보낼수도 있다. 

nowrap 설정에 따라 부모를 넘어서 한줄로 나올수도 있다.



**order**

- 기본 값 : 0
- 작은 숫자 일수록 앞(왼쪽)으로 이동.



**flex-grow**

- 기본 값 : 0
- 주축에서 남는 공간을 항목들에게 분배하는 방법
- 각 아이템의 상대적 비율을 정하는 것이 아님
- 음수는 불가능



**flex-flow**

>  flex-direction 과 flex-wrap 의 shorthand
>
>  `flex-flow: row nowrap;`



------



### 부모 container에 flex 선언시

1. item은 행으로 나열된다.
2. item은 메인축의 시작 선에서 시작한다.
3. item은 크로스축의 크기를 채우기 위해 늘어난다.





+상식

#### Can I Use

#### CSS Flexble Box Layout Module (flexbox)

고차원적인 레이아웃을 할 때 그 브라우저는 항상 확인을 하자.
