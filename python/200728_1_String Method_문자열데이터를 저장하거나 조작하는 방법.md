200728_1_데이터구조1



### 데이터구조

Data Structure란, 데이터를 편리하게 접근하고 변경하기 위해서 `데이터를 저장하거나 조작하는 방법`

컴퓨터 업게의 노벨상을 받으신 분(Niklaus Wirth)의 왈, `프로그램 = 데이터구조 + 알고리즘`으로 구성된다고함.



너무 생소하니 대충 목록 먼저 적어보자.

```
알고리즘에 빈번히 활용되는 순서가 있는(ordered) 데이터 구조
문자열(String)
리스트(List)

데이터 구조에 적용 가능한 Built-in Function
map()
filter()
```



**이 공부할 때 주의사항: **

**조작법(method)의 input이 무엇인지, output이 무엇인지, 원본값을 바꾸는지, 새로운 값을 리턴하는지 꼭꼭 신경써서 공부하기**



### 문자열(String) - 변경할 수 없고, 순서가 있고, 순회가능함

`immutable` `ordered` `iterable`

- .find(x) : x의 첫번째 위치를 반환한다. 없으면 -1반환

> 'apple'.find('p')

- .index(x): x의 첫번째 위치를 반환한다. 없으면 오류발생

> 입력값, 기능 등은 다 find()랑 동일한데, 없는 문자를 넣으면 오류가 발생한다.
>
> 'apple'.index('k')

- .replace(old, new[, count]): 바꿀 대상 글자를 새로운 글자로 바꿔서 반환한다. 조작법: 바꾼다.

> count를 지정하면 해당 갯수만큼만 시행한다.
>
> 'yay!'.replace('a', '_')
>
> 'a'를 '_'로 바꿔서 리턴한다. **리턴값이 있다 = 조작법의 결과를 어떤 특정 변수에 넣었을 때, 그 변수안에 넣은 값을 직접 볼 수 있다.**

- .strip([chars]): 제거할 문자를 지정하면, 양쪽을 제거  `잘 쓰면 코딩테스트 할 때 매우 유용`

> 왼쪽을 제거: lstrip
>
> 오른쪽을 제거: rstrip
>
> 제거할 문자를 지정하지 않으면 공백을 제거
>
> hehehihihihihi'.rstrip('hi')  -> 지정하면 해당 문자열 제거 -> 출력: 'hehe'

- .split(): 특정단위로 문자열을 나누어서 **리스트로 반환** `입력값 받을 때 매우 중요한 애`

>```
>'a_b_c'.split('_')
># 리턴값의 타입은 리스트
># 출력 ['a', 'b', 'c']
>```
>
>
>
>```
># 파이썬 공률
>inputs = input().split()
>print(inputs)
>
>5 3
>['5', '3']
>667
>['667']
>```

- 'separator'.join(iterable) : 원하는 문자열로 만들어서 반환한다. `split가 글자를 리스트로 만들었다면, separator는 리스트를 글자로 만듬` words = ['안녕', 'hello']  /  ' '.join(words) / 출력: '안녕 hello'

> 반복가능한(iterable) 컨테이너의 요소들을 separator를 구분자로 합쳐(join())서 문자열로 반환한다.
>
> word = '배고파' #문자도 이터러블하니까!
>
> '!'.join(word) -> 출력: '배!고!파'

- .capitalize(): 앞글자를 대문자로 만들어 반환

- .title(): 어포스트로피나 공백 이후를 대문자로 만들어 반환

- .upper(): 모두 대문자 만들어 반환

```python
a = 'hI! Everyone, I\'m kim'
print(a.capitalize())
print(a.title())
print(a.upper())

print(a) # 변형을 시켜 반환만 하기때문 a를 출력해보면 그대로 #원본변경NONO
Hi! everyone, i'm kim
Hi! Everyone, I'M Kim
HI! EVERYONE, I'M KIM
hI! Everyone, I'm kim
```

- .lower(): 모두 소문자로 만들어 반환

- .swapcase(): 대문자는 소문자로, 소문자는 대문자로 변경하여 반환

  ```python
  a = 'hI! Everyone, I\'m kim'
  print(a.lower())
  print(a.swapcase())
  print(a)
  hi! everyone, i'm kim
  Hi! eVERYONE, i'M KIM
  hI! Everyone, I'm kim
  ```

- .isalpha(), .isdecimal(), .isdigit(), .isnumeric(), .isspace(), .isupper(), .istitle(), .islower() -기타 문자열 관련 검증 메소드 : 참/거짓 반환

  ```
  이렇게 하면 문자열메소드를 확인할 수 있다.
  dir('string')
  ['__add__',
   '__class__',
   '__contains__',
   '__delattr__',
   '__dir__',
   '__doc__',
   '__eq__',
   '__format__',
   '__ge__',
   '__getattribute__',
   '__getitem__',
   '__getnewargs__',
   '__gt__',
   '__hash__',
   '__init__',
   '__init_subclass__',
   '__iter__',
   '__le__',
   '__len__',
   '__lt__',
   '__mod__',
   '__mul__',
   '__ne__',
   '__new__',
   '__reduce__',
   '__reduce_ex__',
   '__repr__',
   '__rmod__',
   '__rmul__',
   '__setattr__',
   '__sizeof__',
   '__str__',
   '__subclasshook__',
   'capitalize',
   'casefold',
   'center',
   'count',
   'encode',
   'endswith',
   'expandtabs',
   'find',
   'format',
   'format_map',
   'index',
   'isalnum',
   'isalpha',
   'isascii',
   'isdecimal',
   'isdigit',
   'isidentifier',
   'islower',
   'isnumeric',
   'isprintable',
   'isspace',
   'istitle',
   'isupper',
   'join',
   'ljust',
   'lower',
   'lstrip',
   'maketrans',
   'partition',
   'replace',
   'rfind',
   'rindex',
   'rjust',
   'rpartition',
   'rsplit',
   'rstrip',
   'split',
   'splitlines',
   'startswith',
   'strip',
   'swapcase',
   'title',
   'translate',
   'upper',
   'zfill']
  ```

  

  

  

