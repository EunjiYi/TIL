200815에 추가 공부한 내용



### Grid / Responsive Web 그리드시스템, 반응형웹

-------

float : none, left, right



float 없애는 걸 clear라고 부른다. 보통 클래스이름으로 clearfix라고 한다. 

```css
.clearfix::after { /*float를 적용한 부모요소에, clear를 적용한다.*/
    content: "";  /*header tag 다음에, 가상요소로, `내용이 빈` 블록만들기*/
    display: block;
    clear: left; /* float에 left 한걸 없애겠다. 보통 both로 해서 왼쪽 오른쪽 다 없앤다.*/
}
```

-> 이렇게 하면 부모요소 바로 밑에, 보이지 않는 (크기도 모양도 없는) 가상요소가 막고 있다. 그래서 float로 자식이 올라오지 못한다. 

> 부모
>
> `------`
>
> 자식
>
> 이렇게 막히는 꼴



### flexbox 탄생배경

> css layout 관련해서 display, position, float로는 한계가 있다.



* 메인축의 방향 변경: `flex-direction`

  > 기본값 row         `|123 ->` 
  >
  > row-reverse                     `321|<-` 
  >
  > column | 위에서 아래방향 
  >
  > ```
  > -
  > |
  > v
  > 1
  > 2
  > 3
  > ```
  >
  > column-reverse | 아래에서 위 방향
  >
  > ```
  > 
  > 3
  > 2
  > 1
  > ^
  > |
  > -
  > ```
  >
  > 



* 정렬
  * 메인축 방향 정렬: justify(메인축)-content(여러줄)
  * 교차죽 방향 정렬:
    * align-items 한 줄(부모에 설정)
    * align-self: 개별요소(선택된 요소 하나, 자식에 설정)
    * align-content: 여러줄(부모에 설정한다.)

	>justify-content
	>
	>> flex-start (기본값)  |123
	>>
	>> flex-end                                         123|
	>>
	>> center                      |             123        |
	>>
	>> space-between       |1            2          3| (양끝)
	>>
	>> space-around         | 1   2   3 | 외곽간격*2 = 내부간격
	>>
	>> spcae-evenly           |  1  2   3  | 다 동일



> align-items
>
> > flex-start
> >
> > flex-end
> >
> > center
> >
> > stretch
> >
> > baseline : 내부요소들의 폰트사이즈가 다를 때, font를 정렬



> align-content
>
> > flex-start
> >
> > flex-end
> >
> > center
> >
> > stretch
> >
> > space-between
> >
> > space-around



> align-self
>
> > auto
> >
> > flex-start
> >
> > flex-end
> >
> > center
> >
> > stretch
> >
> > baseline



> 기타
>
> > flex-wrap : 부모크기를 넘어서 자식요소들이 나열되면, 
> >
> > > nowrap: 넘쳐도 그냥 둠. 넘쳐도 ok
> > >
> > > wrap:넘치면 아래로
> >
> > flex-flow
> >
> > > flex direction과 flex wrap의 shortend
> > >
> > > 예시: `flex-flow: column wrap;`
> >
> > flex-grow
> >
> > > 각각의 item에(자식들에게) 설정한다. 
> > >
> > > 예시: `flex-grow: 1;`
> > >
> > > order랑 다르게 음수 사용 불가하다.
> >
> > order
> >
> > flex-shrink
> >
> > flex-basis



### flex 시작하기

```
.flex-container {
	display: flex /*부모는 그대로 block속성이다.*/
} /*display: inline-flex 하면 부모도 inline요소가 된다.*/
```

부모에다가 이렇게 주고 시작한다. = flex의 시작!

**이렇게만 설정해도 flex-direction:row가 기본으로 동작. 이게 디폴트값이기 때문**



