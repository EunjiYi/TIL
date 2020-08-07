200720_python_intro



python 공식 문서 

docs.python.org/3/ 

> 파이썬에서 제공하는 스타일 가이드인 PEP-8 내용



#한줄주석

""" ''' 

여러줄 

주석

""" 나 ''' 3개

- 참고로 멀티라인(multiline은 주로 함수/클래스를 설명(docstring)하기 위해 활용한다.)





파이썬은 기본적으로 1줄에 1statment(문장, 파이썬이 실행가능한(executable) 최소한의 코드 단위) 이 원칙지만, 한 줄로 표기를 할 때는 세미콜론`;`을 사용한다.

하지만 잘 쓰지는 않는다.

print('hello'); print('Eunji')

> 여러줄을 작성할 때는 역슬래스`\`를 이용하는데, 잘 쓰는 기능은 아니다.
>
> ```
> print('hello\
> Eunji')
> ```
>
> **PEP-8 가이드 관례(convention)
>
> ```
> print("""hello
> Eunji""")
> #[]{}()는 \ 없이도 가능
> ```
>
> 



리스트 적을 때 양식

lunch = [

​	'배고프다',  '뭐먹지', '행복하지만 어려운 고민'

​	] <= 이렇게 닫히는 괄호를, 첫번째 요소 위치에 오게 하거나 



lunch = [

​	'배고프다',  '뭐먹지', '행복하지만 어려운 고민'

] <= 마지막 줄에서 생성자가 시작되는 첫번째 열에 위치하게 한다.  (권장)





### 할당연산자(Assignment Operator)

type() 데이터타입확인

id() 해당 값의 메모리 주소 확인  ex,)3159049601264



### 식별자(Identifiers) 

>  변수, 함수, 모듈, 클래스 등을 식별하는데 사용되는 name
>
> > 영문알파벳(대소문자-case구별함), 밑줄, 숫자 가능
> >
> > 첫 글자는 숫자 불가능
> >
> > 길이 제한은 없음
> >
> > 내장 함수나 모듈 등의 이름으로 예약어 사용 불가능
> >
> > ```python
> > import keyword
> > print(keyword.kwlist)
> > ```
> >
> > ```py
> > ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
> > ```
> >
> > 



#### 파이썬에서 표현 할 수 있는 가장 큰 수

- 가장 큰 숫자를 활용하기 위해 sys모듈을 불러온다.

- integer(정수 자료형)에서 오버플로우가 없는데, arbitarty-precision arithmetic 를 사용해서다.

  > overflow란 ?:  데이터 타입 별로 사용할 수 있는 메모리의 크기가 제한이 되어 있는데, 표현할 수 있는 수의 범위를 넘어가는 연산을 하면 메모리가 차고 넘친다. 즉, 기대한 값이 출력되지 않는다.
  >
  > > arbitrary-precision arithmetic :
  > >
  > > ```
  > > 파이썬에서 아주 큰 정수를 표현할 때 사용하는 메모리의 크기 변화
  > > 사용할 수 있는 메모리양이 정해져있는 기존의 방식과 달리, 현재 남아있는 만큼의 가용메모리를 모두 수 표현에 끌어다가 쓸 수 있는 형태다. (유동적으로 운용)
  > > ```
  > >
  > > 



수 표현

1. 큰 수 

```python
import sys
max_int = sys.maxsize
# sys.maxsize 의 값은 2**63 - 1 => 64비트에서 부호비트를 뺀 63개의 최대치
print(max_int)
super_max = sys.maxsize * sys.maxsize
print(super_max)
```

출력

```
9223372036854775807
85070591730234615847396907784232501249
```



2. n진수 만들어서 출력하기

```
binary_number = 0b10
octal_number = 0o10
decimal_number = 10
hexadecimal_number = 0x10
print(f"""
2진수 : {binary_number}
8진수 : {octal_number}
10진수 : {decimal_number}
16진수 : {hexadecimal_number}
""")
```

* 8진수 : `0o` / 2진수 : `0b` / 16진수: `0x` 기억하자



3. 컴퓨터식 지수 표현 방식 - e와 E 둘 중 어느 것을 사용해도 무방

   b = 314e-2    # 314 ∗ 0.01 
   print(type(b))
   print(b)

```
<class 'float'>
3.14
```





### 실수 연산할 때 주의하기

__부동소수점을 사용해서 실수를 표현하는데, 항상 같은 값으로 일치되지 않는다. (floating point rounding error)__

__왜냐면 컴퓨터가 2진수(비트)를 통해 숫자를 표현하는데, 그 과정에서 생기는 오류이다. 보통의 경우는 중요하지 않지만 `값을 같은지 비교하는 과정`에서 문제가 발생할 수도 있다! 주의주의하기__

> 해결법 3가지
>
> 1. 약간의 오차는 지수로 무시
>
>    ```
>    abs(a - b) <= 1e-10
>    ```
>
> 2. sys모듈로 처리
>
>    ```
>    import sys
>    abs(a - b) <= sys.float_info.epsilon
>    ```
>
>    >`epsilon` 은 부동소수점 연산에서 반올림을 함으로써 발생하는 오차 상환
>
> 3. python 3.5부터 활용 가능한 math 모듈로 처리
>
>    ```
>    import math
>    math.isclose(a, b)
>    ```



### 표현식에서 주의할 것 몇가지

1. complex(복소수) 적을 때는 띄어쓰기 없어야한다.

   ```
   (o) complex('1+2j')
   (x) complex('1 + 2j') # ValueError
   ```

2. 문자열 표현시 single quotes랑 double quotes, 그리고 여러줄 문장 표현시 """ 사용 가능. PEP-8에서는 하나의 문장부호를 선택하여 유지하라고 말함.

3. 이스케이프문자중에 내가 잘 까먹는것

   ```
   \r : 캐리지리턴
   \0 : null
   ```



두 개 이상의 문자열이 연속해서 나타나면 자동으로 이어 붙여집니다.

'Py' 'thon'

print('Py' 'thon')

출력: Python



#### String interpolation

%-formatting : print('Hello, %s' % name)

str.format(): print('Hello, {}'.format(name))

** 이거쓰자 파이썬 3.6이후 지원**

`f-strings`

 +

그냥 알아두면 유용

```py
import datetime
today = datetime.datetime.now()
print(today)
```

datetime 모듈로 오늘 표현하기 2020-07-26 08:02:58.033843

```python
print(f'오늘은 {today:%y}년 {today:%m}월 {today:%d}일 {today:%A}')
```



divmod는 나눗셈과 관련된 함수입니다.

print(divmod(5, 2))
quotient, remainder = divmod(5, 2)
print(f'몫은 {quotient}, 나머지는 {remainder}')



### 단축평가

- 첫 번째 값이 확실할 때, 두 번째 값은 확인 하지 않음
- 조건문에서 뒷 부분을 판단하지 않아도 되기 때문에 속도 향상



*끄적이기*

-----

 `False`의 반환 형식들

```
0, 0.0, (), [], {}, '', None
```

---

`/` 나눗셈은 항상 float으로 돌려준다.

---

단축평가

```
첫 번째 값이 확실할 때, 두 번째 값은 확인 하지 않음
조건문에서 뒷 부분을 판단하지 않아도 되기 때문에 속도 향상
or 는 하나만 True라도 True이기 때문에 True를 만나면 해당 값을 바로 반환한다.
```

---

연산자 우선순위 - 헷갈리면다 괄호로 묶어버려

1. `()`을 통한 grouping
2. Slicing
3. Indexing
4. 제곱연산자 `**`
5. 단항연산자 `+`, `-` (음수/양수 부호)
6. 산술연산자 `*`, `/`, `%`
7. 산술연산자 `+`, `-`
8. 비교연산자, `in`, `is`
9. `not`
10. `and`
11. `or`

---





형변환

- 암시적 형변환(Implicit Type Conversion)

  사용자가 의도하지 않았지만, 파이썬 내부적으로 자동으로 형변환 
  * bool
  * Numbers (int, float, complex)

- True + 3 =4

- int + float = float

- int + complex = complex



+

추가 상식

### Identity

`is` 연산자를 통해 동일한 object인지 확인

파이썬에서 -5 부터 256 까지의 id는 동일합니다. a is b (a, b가 둘다 5일 때)-> True

257 이후의 id 는 다름 a is b -> False (a, b가 둘다 257일 때)

