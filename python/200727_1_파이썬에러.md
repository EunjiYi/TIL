200727



`^`   캐럿 이라 부름



예외(Exception)

> 실행 도중 예상하지 못한 상황(exception)을 맞이할 때, 프로그램 실행을 멈춤. 

* 문법적으로는 옳지만, 실행시 발생하는 에러.




*  `Exception`을 상속받아 이뤄진다.

> ZeroDivisionError: 0으로 나눌 수 X division by zero
>
> NameError: 정의되지 않은 변수를 호출 name 'abc' is not defined
>
> TypeError: 데이터타입을 혼동, 자료형에 대한 타입 자체가 잘못 .ex.숫자랑 문자 더하려고 할 때. unsupported operand type(s) for +: 'int' and 'str'
>
> > 함수 호출과정에서 TypeError도 발생함. ex.round인데 함수안에 string 넣을 때 round('3.5') type str doesn't define __round__ method
> >
> > 인자 수 가 맞지 않을 때
> >
> > - 인자 적을 때 sample() missing 1 required positional argument: 'k'
> > - 인자 많을 때 choice() takes 2 positional arguments but 3 were given
>
> ValueError:
>
> >자료형에 대한 타입은 올바르나 값이 적절하지 않는 경우  ex.int('3.5') invalid literal for int() with base 10: '3.5'
> >
> >존재하지 않는 값을 찾고자 할 경우 3 is not in list
>
> IndexError 존재하지 않는 index로 조회할 경우 ex. 비어있는 리스트 제일 끝 값 가져오라 할 때 listlist[-1]  list index out of range
>
> 
>
> KeyError 딕셔너리에서 키가 없을 때 -> 에러 예시: 'queen' (없는 키 이름)
>
> ModuleNotFoundError: 모듈을 찾을 수 없을 때  No module named 'reque'
>
> ImportError: 모듈을 찾았으나 가져오는 과정에서 실패하는 경우, 존재하지 않는 클래스나 함수를 호출할 때
>
> >from random import sampl
> >
> >-> cannot import name 'sampl' from 'random'  (c:\users\eunji yi\appdata\local\programs\python\python37\lib\random.py)
>
> 
>
> KeyboardInterrupt 코드 잘 돌아가고 있는데 ctrl+c 눌렀을 때
>
> ```python
> ---------------------------------------------------------------------------
> KeyboardInterrupt                         Traceback (most recent call last)
> <ipython-input-28-8b9466abc919> in <module>
>       2 # =====
>       3 while True:
> ----> 4     continue
> 
> KeyboardInterrupt: 
> ```
>
> 





