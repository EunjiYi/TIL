200729_2_OOP(객체지향프로그래밍)

개념이 좀 어렵지만 확실히 이해하고나니까 별로 적을게 없다. (그래놓고 겁나 많이 적음)



`oop1`

# 객체(Object) : 물체라 생각하자

> Python에서 **모든 것은 객체(object)**이다.

> 모든 객체는 **타입(type)-데이터타입, 속성(attribute), 조작법(method)-방법**을 가진다.
> 파이썬은 모든 것이 객체이다. 타입, 속성, 조작법을 지니는 덩어리가 객체이다.



### 객체(Object)의 특징

- **타입(type)- 특정한 데이터들은 걔들끼리만 묶어서 할 수 있는 연산자가 있다. 공통적인 속성(특성)을 가진 애들, 분류될 때 구분자**: 어떤 연산자(operator)와 조작(method)이 가능한가? 

  >타입(Type)
  >
  >- 공통된 속성(attribute)과 조작법(method)을 가진 객체들의 분류
  >- 우리가 정의한 새로운 분류체계를 만들어내기위해 = 객체지향프로그래밍의 완성, 유연하게 프로그래밍이 됨

- **속성(attribute)**: 어떤 상태(데이터)를 가지는가? ex.물의 속성, 모니터의 속성 중 색깔은 검정색이다. - 속성(상태)을 컴퓨터에선 데이터로 표현한다.

  > 특정 데이터 타입(또는 클래스)의 객체들이 가지게 될 상태/데이터를 의미

- **조작법(method)**: 어떤 행위(함수)를 할 수 있는가? ex.리모컨 조작법



#### 타입(Type)=(공통속성을 가진 애들을 분류하는 기준.)과 

#### 인스턴스(Instance)=(예시, 그 특정 타입에 속하는 객체).

> 인스턴스
>
> - 특정 타입(type)의 실제 데이터 예시(instance)이다.
> - 파이썬에서 모든 것은 객체이고, **모든 객체는 특정 타입의 인스턴스**이다.
> - (가라)그냥 쉽게 객체를 인스턴스라고 생각해도 무관
> - isinstance(데이터, 타입) ex. a가 int의 인스턴스인가? isinstance(a, int)

객체는 타입이 있고, 인스턴스는 그 타입의 하나의 예시
각각의 타입마다 각각 사용할 수 있는 연산자가 다르다. 동작이 다르다. >ex. string 타입이면 +연산자가 왔을 때 두 문자를 합쳐준다



#### 속성(Attribute)=(특징:characteristic)과 메서드(Method)

* 객체의 속성(상태, 데이터)과 조작법(함수)
  `set은 별도의 메소드가 없다. `

  - 속성(attribute): 객체(object)의 상태/데이터를 뜻한다. .ex복소수는 실수속성과(.real)와 허수속성(.imag)을 가진다. = `complex` 타입 인스턴스가 가진 속성

  - 메서드(Method): - 특정 객체에 적용할 수 있는 행위(behavior). 특정 데이터 타입(또는 클래스)의 객체에 공통적으로 적용 가능한 행위(behavior)들을 의미

    ​								- <u>클래스에 정의된 함수</u> #이거조심 까먹지말자. 함수랑 메소드 구분하자.

    >  dir 이라는 내장함수
    >
    >  `list가 가지는 속성과 쓸 수 있는 메소드가 모두 나타난다. `
    >
    > 
    >
    > print(dir(list)) 아니면 dir([])
    >
    >  `list 타입의 객체들이 할 수 있는 것들을 알려줌. `
    >
    >  `list 타입 객체가 가지고 있는 모든 속성과 메서드를 보여준다.
    > `
    >
    > 
    >
    >  `앞뒤에 `__`로 되어있는 것들은 매직메소드라고 한다.
    >  list에서 +를 만났을 때 그 결과를 	__add__	에 넣어준다.



-------------



### 객체 지향 프로그래밍(Object-Oriented Programming)

`#OOP로 코드를 짜게되면 코드가 굉장히 직관적이 된다.`

`#OOP의 굉장한 장점 : 딱 이 한 줄만 봐도 코드가 뭘 하려는지 알겠음`

Object가 중심(oriented)이 되는 프로그래밍 - 객체를 만들면서 프로그래밍을 한다.



**<by wikipedia for 객체지향 프로그래밍>**

>
>객체 지향 프로그래밍(영어: Object-Oriented Programming, OOP)은 컴퓨터 프로그래밍의 패러다임의 하나이다. 
>
>객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다.



절차 중심 vs. Object 중심

> *프로그래밍 패러다임*: 어떻게 프로그램을 정돈(organize)할 것인가 - how to organize certain programinig



---------------------



# 클래스(Class)와 객체(Object)

> class = '분류', 타입으로 구분될 수 있는 무언가를 하는 것
> 클래스와 타입은 동일한 의미
> `type`: 공통 속성을 가진 객체들의 분류(class)
> `class`: 객체들의 분류(class)를 정의할 때 쓰이는 키워드 # 타입과 클래스가 동일한 개념이라고 보면됨.

- 굳이 클래스라는 이름으로 구분해 놓은 이유: 그동안 우리는 파이썬에서 만들어놓은 타입(객체의 종류)들만 사용했었다, 이제는 내가 직접 나만의 분류(타입)을 정의할거다 그때, 프로그래밍키워드로서'클래스'라는 용어를 쓴다. = 나는 이런 '분류'를 만들거야, 이런 '타입'을 만들거야



## 클래스(Class) 생성

* `<클래스의 이름>`은 `PascalCase(어퍼캐멀케이스) = 대문자대문자`로 정의한다. 

  * cf)cascalCase 소문자대문자(캐멀케이스)

* `is_odd()` =  Snake Case 여러 단어로 이루어진 단어 사이를 언더바(_)로 나누는 방식

  

//  메소드와 함수의 차이 //

* 클래스 내부에는 데이터와 함수를 정의할 수 있고, 이때 정의된 함수는 **메서드(method)**로 불린다.



### 인스턴스(Instance) 생성

* 정의된 `클래스(class)-필름` 에 속하는 객체를 해당 클래스의 `인스턴스(instance)-인화한 사진`라고 한다.

> 인스턴스 = 클래스()
>
> person1 = Person()
>
> > `person1`은 사용자가 정의한(user-defined) `Person이라는 데이터 타입(data type) = 클래스의 인스턴스`이다.
> >
> > 타입(Person이라는 타입)을 정의한다 = 객체에게 공통적인 속성과 메소드를 부여한다. 
>
> `type()` 함수를 통해 생성된 객체의 클래스(타입)를 확인가능
>
> > print(type(person1))  => 출력 <class '__main__.Person'>



#### 클래스설명서

```python
class Person:
    #멀티라인스트링 = doc string = __doc__ = document string
    """ #해당 클래스의 대한 설명을 적는 document = __doc__ 
    이것은 Person 클래스(class)입니다. ㅎㅎㅎ 
    속성은 뭐가 있고
    메소드는 무엇무엇이 있습니다. 
    """
```

```python
person1 = Person()
person2 = Person()
print(type(person1))
print(type(person2))
print(person1.__doc__) # 다른 사람이 이 클래스가 뭔지 모를 때 이렇게 코드 치면, 클래스 설명이 나온다. 
print(person2.__doc__) # 다른 사람을 위한 클래스 설명서
```



### `__main__`꿀팁

```python
if __name__ == "__main__":
    pass

# __name__ 값이 들어갈 때 = python 파일명.py를 직접 호출했을 때
__name__ = "__main__"

# 모듈로 import로 불러져 오거나 다른 곳에서 호출이 된 경우
__name__ = 호출된 파일명
# 직접 파일이 실행이 되었는지 다른 곳에서 호출이 된 것인지를 __name__을 보고 알 수 있는 것이다. 
```



### 이것도 꿀팁

```
파이썬에서 기본적으로 정의돼있는 클래스 = integer, list, dictionary, boolean 등

li = [1,2,3,]
print(type(li))
help()
#help()별거 아닌데 왜이리 기억을 못하고 자꾸 까먹냐 너는
```





### 메서드 정의 

### *중요*

```
#
list.sort() #리스트가 가지고 있는 데이터를 변경 
#리스트가 자기 자신을 정렬하는 일을 sort()라는 메소드로 정의해놓은 것이다. 

#객체지향 - 메소드를 호출하고 자기자신에 대한 행위를 한다.
list.sort()

#절차지향 - 인풋 아웃풋으로 데이터가 흐른다.
a = sorted(list)

# 함수를 정의해서 프로그래밍 하는것 = 굳이 말하면 절차지향이다. 
# 함수자체가 속성, 메소드, 타입이 있는(=객체) 형태는 아니잖아. 객체지향은 데이터(속성)가 하는 일이 전부 클래스 안에 다 정의되어있어야한다.
# 파이썬 자체가 객체지향언어이기는 하지만 함수 만드는 것은 대표적인 절차지향언어인 c도 함.
```

help(float) 

dir(Person)



### 생성자(constructor) 메서드 - 메서드 정의 시에 인자로 self를 붙여줌
인스턴스 객체가 생성될 때 호출되는 함수.    
> 태어날 때부터 가져야할 속성들을 같이 넣어서 만들어주면 좋겠지?
>
> 생성자를 활용하면 인스턴스가 생성될 때 인스턴스의 속성을 정의할 수 있다.
```python
def __init__(self): #메소드에 일단 self라고 넣고 보자
    print('생성될 때 자동으로 호출되는 메서드입니다.')
```



### 소멸자(destructor) 메서드 - 메서드 정의 시에 인자로 self를 붙여줌
인스턴스 객체가 소멸(파괴)되기 직전에 호출되는 함수.

> 가비지 컬렉터가 필요없어졌을 때 지워준다. 그 때 소멸자 메소드 호출됨. 그래서 갈게;.. 나오는 것.
```py
def __del__(self):
    print('소멸될 때 자동으로 호출되는 메서드입니다.')
```



```python
class Person:
    def __init__(self):  #생성자
        print('응애!')
        
    def __del__(self):  #소멸자
        print('갈게..')

p1 = Person()  #생성
응애!

del p1  #소멸
# 지우면 객체가 사라지게 되고 사라지는 시점에 소멸자메소드 호출
갈게..

p1 = Person() #가비지콜렉터
p1 = 'hello'
응애!
갈게..
```



### 속성의 정의, 활용법

인스턴스의 속성 = 개별 인스턴스들이 사용할 데이터

```python
class Person:
    def __init__(self, name):  # 객체 스스로는 담아줌.
        self.name = name
```

클래스의 인스턴스 뿐만 아니라 **클래스 자체도 속성을 지닐 수 있다.** 
**속성도 생성자를 통해 정의가 가능하다.**



------------------------------



## 매직메서드/스페셜메소드
- 더블언더스코어(`__`)가 있는 메서드는 특별한 일을 하기 위해 만들어진 메서드이기 때문에 `스페셜 메서드` 혹은 `매직 메서드`라고 불린다.

* 우리가 정의한게 아니라 파이썬이 미리 정의를 해놓고 일련의 규칙들을 만들어 놓음
  뭔가가 __로 시작한다 ! : 내부적으로 파이썬 스스로가 활용하기 위해서, 파이썬 자체의 기능을 활용하는 아이들임. 

- 매직(스페셜) 메서드 형태: `__someting__
```
dir() # 아무것도 안 들어가 있는 함수 호출, 굳이 따지면 None에 가까움. 얘의 defualt 알규먼트가 뭐에 가까운지는 문서를 보면 되지.

# 아무것도 안넣으면 파이썬이 기본적으로 가지고 있는 일반적인 객체에 대한 정보들이 나옴 
dir('') # dir함수의 인자로 문자열을 넣은 것 , 글자를 넣으면 글자에 적용할 수 있는 애들이 나오고
dir(1) # 숫자를 넣으면 숫자에 적용할 수 있는 애들이 나오고
```



```
3.14.__add__(2.5) #3.14+2.5 # 파이썬의 모든 것은 객체니까.
#---------------------------------------------------------

# concatenation
'hello' + 'ssafy'
'hello'.__add__('ssafy')
둘다 같은 결과

#---------------------------------------------------------
p1 = Person()
p2 = Person()
#매직메소드를 활용해서 사용자가 지정한 클래스도 연산가능하다. 
p1 + p2

#---------------------------------------------------------
dir('') 해서 나오는 것들 중
__contains__ = in
__eq__ = ==
__ge__ = >= 크거나 같다.
```





### `__str__(self)` 

> 특정 객체를 출력(`print()`) 할 때 보여줄 내용을 정의할 수 있음

```py
class Person:
    def __str__(self):
        return '객체 출력(print)시 보여줄 내용'
```



까먹지마

dir() 함수를 통해 특정 객체가 활용 가능한 메서드를 확인

dir(Person)

```python
이런 클래스가 있을 때, 인스턴스 person1을 생성하고 출력하면 이렇게 나옴
class Person:
    def __init__(self, name):
        self.name = name

p1 = Person('john')
print(p1)  #출력: <__main__.Person object at 0x00000209A975E608> 

#-> 이 결과를 사람이 알아보기 쉽게 해보자   
# __str__() 매직메서드를 정의해보자
class Person:
    def __init__(self, name):
        self.name = name
    
    def __str__(self): #문자열로 형변환을 했을 때 어떤 모습을 가질지 정의하자.
        return f'나는 {self.name}'

p1 = Person('john')  -> 출력: 나는 john
```

```
#
print(p1) #프린트로 예쁘게 나오는 애들은 다 형변환이 가능한 애들이다. 리스트도 문자열로, 딕셔너리를 쓰더라도 문자열로 출력
#str() : 프린팅할 때 매우 중요한 펑션, 문자열로 만들어줌. 형변환
str([1,2,3]) #프린트로 예쁘게 나오는 애들은 다 형변환이 가능한 애들이다.
```





###  `self` : 인스턴스 자신(self)

* Python에서 객체의 메서드는 **호출 시 첫번째 인자로 인스턴스 자신이 전달**되게 설계되었다. 

  ```python
  eunji = Person('eunji')
  eunji.talk()
  결과랑
  Person.talk(eunji)
  결과랑 
  둘다 출력이 
  '안녕, 나는 eunji'  #헐 이게 뭐야
  ```

  


* 보통 매개변수명으로 `self`를 첫번째 인자로 설정(다른 이름도 가능)

```
eunji.talk()
#eunji라는 인스턴스가 talk라는 메소드를 부를 때, 알게모르게 **해당하는 함수의 첫번째 인자로 자기 스스로를 집어넣게**된다.
#파이썬 자체 기능.
```



---------

----------

----------



`oop2`

### 인스턴스 변수와 클래스 변수

* 인스턴스 변수  = 각 인스턴스들의 고유한 변수(데이터) = 인스턴스의 속성(attribute)

  > - 메서드 정의에서 `self.변수명`로 정의
  > - 인스턴스가 생성된 이후 `인스턴스.변수명`로 접근 및 할당

```python
# 각자 이름을 가졌다는 것은 공통점인데, 이름 자체의 값은 다르다. = 인스턴스 변수
john = Person('john')
eric = Person('eric')

print(john.name)  -> john
print(eric.name)  -> eric
```



* 클래스 변수 = 클래스의 속성(attribute)

  > - 해당 클래스의 모든 인스턴스가 공유, 접근가능
  >
  >   ```python
  >   class Person:
  >       species = 'human' #john이 접근해도 eric이 접근해도 human
  >       #클래스가 공유하는 변수 = 클래스변수, 객체에서 클래스 변수에 접근/재할당
  >   print(john.species) # => human  #자기자신만의 공간 + 클래스의 공간을 쉐어하는 느낌
  >   print(eric.species) # => human
  >   #여기서 이렇게 하면, 
  >   john.species = 'developer'
  >   print(john.species) # => developer  #john만 접근할 수 있는 본인만의 데이터
  >   print(eric.species) # => human
  >   
  >   
  >   # Q.클래스변수 접근하는 법
  >   # person이라는 클래스로부터 접근
  >   # species로는 조회가 안되지만 #Person.sepcies로 하면 조회가 된다! 클래스가 모듈처럼 작동하는 격
  >   print(Person.species) # => human
  >   
  >   Person.talk(john) 
  >   # 마치 클래스를 이름공간처럼 활용해서 Person 타고 들어와서 def talk(self) = Person공간안에있는 talk를 부른것과 동일
  >   ```
  >
  > - 클래스 정의 내부에서 선언  **인스턴스변수는 메서드 정의할 때 sel.변수명으로 하고, 클래스 변수는 걍 클래스 내부에 선언하면 됨.
  >
  > - `클래스.변수명` 또는 `인스턴스.변수명`으로 접근(할당)





### 인스턴스 메서드(instance method)

> - 인스턴스가 사용할 메서드 -> 클래스 전체가 공유하는게 아니고 인스턴스 자체가 자기 스스로 사용하는 것. 인스턴스 변수처럼
> - 클래스 내부에 정의되는 메서드의 기본값은 인스턴스 메서드
> - **호출시, 첫번째 인자로 인스턴스 자기자신 `self`이 전달됨**

```python
class MyClass:
    def instance_method(self, arg1, arg2, ...):
        ...

my_instance = MyClass()
my_instance.instance_method(arg1, arg2)

# 호출시, 첫 번째 인자로 인스턴스(my_instance)가 전달됨
MyClass.instane_method(my_instance, arg1, arg2) 
```



### 클래스 메서드(class method)

> * 클래스가 사용할 메서드
> * `@classmethod` 데코레이터를 사용하여 정의
> * **호출시, 첫 번째 인자로 클래스 `cls`가 전달됨** -> 솔직히 이건 self로 하던 다른 이름으로 하던 상관없는데 서로서로 알아볼 수 있게 cls로 한다!

```python
class MyClass:
    @classmethod
    def class_method(cls, arg1, arg2, ...):
        ...

# 호출시, 첫 번째 인자로 클래스(MyClass)가 전달됨
MyClass.class_method(MyClass, arg1, arg2, ...)  
```



### 스태틱 메서드(static method) = 정적 메서드

> - 클래스가 사용할 메서드
>
> - `@staticmethod` 데코레이터를 사용하여 정의
>
> - **호출시, 어떠한 인자도 전달되지 않음** -> 이게 클래스 메서드와 다른점.
>
>   >  클래스 메서드는 메서드 안에서 클래스 속성, 클래스 메서드에 `접근해야 할 때 `사용하며, 
>   >
>   > 그렇지 않을 경우 정적 메서드를 사용

```python
class MyClass:
    @staticmethod
    def static_method(arg1, arg2, ...):
        ...

# 호출시, 어떤 인자도 전달되지 않음
MyClass.static_method(arg1, arg2)
```





### 주의사항

- 인스턴스는, 3가지 종류의 메서드 모두에 접근할 수 있다. 하지만 인스턴스에서 클래스 메서드와 스태틱 메서드는 호출하지 않아야 한다.` (가능하다 != 사용한다)`
  - `인스턴스가 할 행동은 모두 인스턴스 메서드로 한정 지어서 설계한다.`



- 클래스 또한 3가지 종류의 메서드 모두에 접근할 수 있지만, 클래스에서 인스턴스 메서드는 호출하지 않는다. `(가능하다 != 사용한다)`

  - 클래스 메서드와 스태틱 메서드는 <u>인스턴스 없이 호출할 수 있다는 점은 같다.</u>

    > 클래스 자체(`cls`)와 그 속성(클래스변수)에 접근할 필요가 있다면 **클래스 메서드**로 정의한다.

    > 클래스와 클래스 속성에 접근할 필요가 없다면 **스태틱 메서드**로 정의한다.





-----------------------

-------------

----------

------



`oop3`

#### 상속 Inheritance = 부모 클래스의 모든 속성이 자식 클래스에게 상속

> 코드 중복 ㄴㄴ
>
> 공통된 속성이나 메소드를 부모클래스에 정의하고 상속함으로써, 적은코드로 다양한 형태 객체 만들기 가능
>
> 상속관계 확인 issubclass(Student, Person)
>
> > issubclass(bool, int) # True
> > issubclass(float, int) # False  -> 확인하는 법 help(bool)  찍어보기

*참고* 

	s는 Student의 인스턴스다. = True
	print(isinstance(s, Student), isinstance(s, Person))  -> 둘다 True



 헷갈리면 안돼!

```python
class Person:
    population = 0
    
    def __init__(self, name='사람'):
        self.name = name
        Person.population += 1
        
    def greeting(self):
        print(f'반갑습니다. {self.name}입니다.')
        
class Student(Person):
    def __init__(self, student_id, name='학생'):
        self.name = name
        self.student_id = student_id  #부모클래스의  Personr과의 차이
        Person.population += 1

s = Student(1)py
print(s.student_id) -> 1
print(Student.population) -> 2
print(Person.population) -> 2
```

#여기서는 Student.population랑 Person.population랑 같은것.

**그러면 Student.population은 인스턴스(Student)로 클래스(Person)변수(population)에 접근한 것.**!



------------------



## `super()`

* 자식 클래스에 메서드를 추가로 구현할 수 있는데, 부모 클래스의 내용을 사용할거면`super()`를 쓴다.



이거 헷갈림 ㅜ 제대로 알자.

```python
#상속을 했음에도 불구하고 동일한 코드가 반복된다.
class Person:
    def __init__(self, name, age, number, email):
        self.name = name
        self.age = age
        self.number = number
        self.email = email 
        
    def greeting(self):
        print(f'안녕, {self.name}')
      
    
class Student(Person):
    def __init__(self, name, age, number, email, student_id):
        self.name = name
        self.age = age
        self.number = number
        self.email = email 
        self.student_id = student_id
        
p1 = Person('홍길동', 200, '0101231234', 'hong@gildong')  #p1은 Person의 인스턴스
s1 = Student('김싸피', 20, '12312312', 'student@naver.com', '190000')  #s1은 Student의 인스턴스
```

```python
##Student의 부모클래스 생성자가 호출됨.
class Person:
    def __init__(self, name, age, number, email):
        self.name = name
        self.age = age
        self.number = number
        self.email = email 
        
    def greeting(self):
        print(f'안녕, {self.name}')
        
        
class Student(Person):
    def __init__(self, name, age, number, email, student_id):
        # Person()
        super().__init__(name, age, number, email) #Student의 부소클래스 생성자가 호출됨.
        self.student_id = student_id
        
        
p1 = Person('홍길동', 200, '0101231234', 'hong@gildong')
s1 = Student('김싸피', 20, '12312312', 'student@naver.com', '190000')

p1.greeting()
s1.greeting()
안녕, 홍길동
안녕, 김싸피
```



`+` 생소하니까 예제 하나 더

###  Rectangle & Square

```python
#
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

    def perimeter(self):
        return 2* (self.length + self.width)
```

```python
# Square 클래스는 Rectangle 클래스에서 상속받은 속성 외 추가 속성을 가지고 있지 않습니다.
class Square(Rectangle):
    def __init__(self, length): #생성자로 값을 하나만 받아도 정사각형이니까 상관이 없다.
        super().__init__(length, length)
```



------------



# 메서드 오버라이딩 Method Overriding
> Method Overriding(메서드 재정의): 자식 클래스에서 부모 클래스의 메서드를 재정의하는 것
>
> 상속 받은 메서드를 `재정의`할 수도 있는데, 상속 받은 클래스에서 **같은 이름의 메서드**로 덮어쓴다.





#### 파이썬은 오버로딩을 지원하지 않는다.

파이썬은 가장 마지막에 선언된 메소드만 남아있어서 그 위에 greeting(self)는 무용지물
        #오버로딩쓰고 싶으면 파이썬 패키지 받으면 되긴하는데 굳이?

> 하나의 클래스 내부에서 메소드의 이름은 같은데, `받은 인자의 종류나 갯수가 달라서 같은 이름의 메소드를 여러개 만들어주는 것` = `오버로딩` = 파이썬은 공식적으로 지원하지 않는다. 메소드 오버라이딩만 지원한다. 왜냐면 파이썬은 같은 이름으로 메소드가 정의 되면 계속 덮어씌워지니까.











#### 다중 상속(Multiple Inheritance)

두개 이상의 클래스를 상속받는 경우

 상속의 순서가 중요.(왼쪽에서 오른쪽)

```python
#
class Mom(Person):
    gene = 'XX'
    
    def swim(self):
        return '첨벙첨벙'

    
class Dad(Person):
    gene = 'XY'
    
    def walk(self):
        return '성큼성큼'
    
#    
class FirstChild(Dad, Mom):  # gene은 Dad의 gene값을 가져옴.
    def swim(self):  # Mom의 swim 메서드를 overriding - Dad에는 swim()이 없으니까. #override 된 Child의 swim이 호출됨
        return '챱챱'
        
    def cry(self):  # Child 만이 가지는 인스턴스메서드 
        return '응애'
```



양도 많고 개념도 어렵지만 하고나니 매우 뿌듯하구나. 객체지향프로그래밍 오오피