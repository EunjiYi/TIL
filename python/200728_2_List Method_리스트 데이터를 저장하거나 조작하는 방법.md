200728_2_data_structure_I

데이터 구조로서의 리스트(list)와 조작법(method)



### 리스트(List) - 변경가능하고, 순서가 있고, 순회가능함 

`mutable` `ordered` `iterable` 

### 값 자체가 변경이 가능하니까 조작법이 좀 달라진다.

원본을 변경하는 것이 된다. 리턴값이 None



* .append(x): 리스트에 값을 추가한다.

  > cafe.append('banapresso')
  > print(cafe) -> ['starbucks', 'tomntoms', 'hollys', 'banapresso']
  >
  > new_cafe = cafe.append('banapresso')
  >
  > print(new_cafe) -> None #응 조심해

* .extend(iterable): 리스트에 iterable(list, range, tuple, string**[주의]**) 값을 붙인다.

  > [1,2,3] + [4,5,6]  -> [1,2,3,4,5,6] #이 덧셈의 역할이 완벽하게 extend의 기능
  >
  > cafe += ['mc_cafe', 'droptop']  # 이거랑 extend랑 완벽히 같음 ( list concatenate 동일)
  >
  > cafe.extend(['mc_cafe', 'droptop])

* 어팬드랑 달라!

  ```python
  cafe.append(['coffeenie'])
  cafe.append('coffeenie')
  print(cafe)
  print('-----')
  
  cafe.extend(['twosome_place'])
  #이러면 핵망 cafe.extend('twosome_place') 
  #  't', 'w', 'o', 's', 'o', 'm', 'e', '_', 'p', 'l', 'a', 'c', 'e' 이렇게 들어감
  print(cafe)
  
  ['starbucks', 'tomntoms', 'hollys', 'banapresso', 'banapresso', 'wcafe', '빽다방', 'mc_cafe', 'droptop', ['coffeenie'], 'coffeenie', 'twosome_place', ['coffeenie'], 'coffeenie']
  -----
  ['starbucks', 'tomntoms', 'hollys', 'banapresso', 'banapresso', 'wcafe', '빽다방', 'mc_cafe', 'droptop', ['coffeenie'], 'coffeenie', 'twosome_place', ['coffeenie'], 'coffeenie', 'twosome_place']
  ```

  >
  >
  >cafe.extend('ediya') #글자도 이터러블 = 순서가 있는 자료형
  >#이게 이런 느낌으로 들어간거다. cafe.extend(['e', 'd', 'i', 'y', 'a'])
  >print(cafe)

* .insert(i, x): 정해진 위치 i에 값을 추가한다. 원본 데이터를 수정했기 때문에 리턴값이 없다. 

  >cafe.insert(len(cafe)+100, '!')
  >
  >리스트의 길이를 넘어서는 인덱스는 마지막에 아이템이 추가됨. 얘는 인덱스에러(out of range) 안나고 마지막에 추가됨. 아 왜 다 통일되지 못한 것이냐

* .remove(x): 리스트에서 값이 x인 것을 삭제한다.

  > #원본변경이라 리턴이 없다. 
  >
  > numbers.remove(1) 
  >
  > #값이 없으면 오류가 발생  #없앨 값이 없으면 벨류에러
  >
  > ValueError: list.remove(x): x not in list

* .pop(i): 정해진 위치 i에 있는 값을 삭제하며, 그 항목을 반환한다. `i`가 지정되지 않으면 마지막 항목을 삭제하고 반환.

  > 리턴값이 있다.!! = 별도의 변수에 저장할 수 있다
  >
  > a = [1, 2, 3, 4, 5, 6]
  >
  > print(a.pop(0)) - > 1
  > print(a) -> [2, 3, 4, 5, 6]

* .clear(): 리스트의 모든 항목을 삭제함 -> 출력 []

* .index(x): x값을 찾아 그 index를 반환

  > 없을 시 오류가 발생 = remove 역시도 같은 에러가 발생
  >
  > 100 is not in list

* .count(x): 원하는 값의 개수를 확인한다.

  > [응용] 원하는 값을 모두 삭제
  >
  > a = [1, 2, 1, 3, 4]
  > target_value = 1
  > for i in range(a.count(target_value)):
  >     a.remove(target_value)
  >
  > -> 출력 [2,3,4]

* .sort(): 정렬, 원본list를 변형시키고 None을 리턴.  `내장함수 sorted()랑 비교`

  > import random
  >
  > lotto = random.sample(range(1, 46), 6)
  > print(lotto) -> [38, 11, 7, 37, 20, 33]
  >
  > lotto.sort(reverse=True) -> [38, 37, 33, 20, 11, 7]

* reverse(): 반대로 뒤집는거, `정렬아님!!`

  > classroom.reverse()



두 개가 아니라 두 변수(copy_list랑 orginal_list) 둘다 같은 한 리스트([1,2,3]를 바라보고 있었다. 

확인법 = id 가 같다.

id(copy_list) == id(original_list)
copy_list is original_list

-> True



** 리스트 메소드들은 원본을 변경하지! 그러니까 리스트를 복사하고 싶다면?**

### mutable 데이터의 복사는 어떻게 이루어질까?

`list` `dict` `set`

1. slice 연산자를 사용 [:]

   ```python
   a = [1, 2, 3]
   b = a[:] # b=a라고 하면 같은 걸 가리키니까. 슬라이스오퍼레이터는 원본변경이 아닌 새로운 값을 리턴한다. 
   #print(b)
   
   b[0] = 5
   print(a) # 이렇게 하면 b를 변경해도 a는 변경되지 않음
   ```

2. list() 활용

   ```python
   a = [1, 2, 3]
   b = list(a) # 새로운 리스트 생성 
   
   b[0] = 5
   print(a)
   ```

   ```python
   +
   a = [1, 2, 3]
   b = list(a) # 이게 메모리 측면에서 효율적!
   
   a = [1, 2, 3]
   b = []
   b += a #이러면 b라는 빈 리스트는 리스트대로 할당받고, a가 할당된 곳에서 값을 b로 옮겨주기까지해서 메모리가 더 많이 쓰임 (with 승한)
   ```

   

하지만, 이렇게 하는 것도 일부 상황에만 서로 `다른 얕은 복사(shallow copy)`이다.

	>  shallow copy : 슬라이싱오퍼레이터쓰던가, list()쓰는 것.	

```py
# python tutor로 돌려서 왜 [1,2]는 복사 안되는지 보기
# a[2]는 실제 값인 [1,2]가 들어있는게 아니라, 리스트[1,2]를 가리키는 주소값이 들어있어서 그런다.
a = [1, 2, [1, 2]]
b = a[:]

b[2][0] = 3
print(a) -> [1, 2, [3, 2]]
# 그래서 프린트 a를 해도 b에서 변경한 값이 들어감. 
#이것은 shallow copy를 통해 새로운값으로 만드는 수혜를 못받음.
```



3. 그래서 딥카피를 해야함. 나중에 실제로 구현할 거임.(재귀적)
   import copy

   a = [1, 2, [1, 2]]
   b = copy.deepcopy(a)

   b[2][0] = 3
   print(a) -> [1, 2, [1, 2]]

`deep copy` : 만일 중첩된 상황에서 복사를 하고 싶다면, `깊은 복사`를 해야한다. 즉, 내부에 있는 모든 객체까지 새롭게 값이 변경된다.





### 매우중요한 List Comprehension

> List Comprehension: 표현식과 제어문을 통해 리스트를 생성, 여러 줄의 코드를 한 줄로 줄일 수 있다.
>
> ```python
> [식 for 변수 in iterable]
> 
> list(식 for 변수 in iterable)
> ```

cubic_list = [ number ** 3 for number in numbers]



> ```python
> [식 for 변수 in iterable if 조건식]
> 
> [식 if 조건식 else 식 for 변수 in iterable]
> 
> # elif 는 다음과 같이 사용해야 합니다. (if else 열거)
> [식 if 조건식 else 식 if 조건식 else 식 if ... else ... for 변수 in iterable]
> ```

순서

1. 반복문을 먼저 짠다. (# 3. 조건식을 건다.)

2. 반복문을 리스트에 들고 온다.

   

1~10까지의 숫자 중 짝수만 담긴 리스트 만들기

even_list = [number for number in range(1,11) if number % 2 == 0]
	



----------

----------

#### 순회가능한(iterable) 데이터 구조에 적용가능한 Built-in Function

> iterable : `list`, `dict`, `set`, `str`, `bytes`, `tuple`, `range` 알아도 또 보고 또 보자

- `map()`
- `filter()`
- `zip()`
- ~~`reduce()`~~ ([참고](https://docs.python.org/ko/3/library/functools.html#functools.reduce))



_내가 잘 모르고 생소하므로 예제도 가능한 세세히 적어보겠다._

1_`map(function, iterable)` 

* 입력값을 받아서, 리스트에 들어있는 요소에 공통적인 오퍼레이션을 적용할 때 사용.



* return은 `map_object` 형태이다.

  

  문제: numbers = [1, 2, 3] ->문자열 '123'으로 만드세요

  생각: 리스트를 글자로 변형시키네, 그러니까 .join 쓸수 있다. 

   * numbers = ['1','2','3']
         ''.join(numbers)  


  * numbers = [1, 2, 3] 이걸로 직접적으로는 안됨. 그러니  numbers의 요소를 int에서 str로 바꾸자 

    
  * new_numbers = ''.join([str(num) for num in numbers])
    
        

### 풀이: 이걸 map쓰면 매우 간단해짐

  ​			map(공통적으로 적용할 함수, 순회가능한 자료)
  ​			`new_numbers = map(str, numbers) `

​				print(new_numbers)   -> 출력: <map object at 0x0000018C9F936948>
​				print(list(new_numbers)) # 리스트로 쉽게 만들어서 쓰면 된다. ->출력: ['1', '2', '3']

​				print(''.join(new_numbers)) ->출력 123

  ​			**주의 str(number)처럼 함수의 호출을 하지 말고, 함수의 이름을 적어라. 사용자정의함수도 가능하다.** 

  ​			`map()` 함수는 입력값을 처리할 때 자주 활용된다.

​			반대로 numbers = ['1', '2', '3'] -> 숫자123 으로 만드려고 하면, 

​			new_numbers = map(int, numbers)
​			new_numbers = list(map(int, numbers)) 아니면

​			new_numbers = [int(num) for num in numbers]





2_`filter(function, iterable)`

* iterable에서 function의 반환된 결과가 `True` 인 것들만 구성하여 반환한다.


* `filter object` 를 반환한다.

```python
def odd(n):
    return n % 2
#이거랑
new_numbers = list(filter(odd, numbers))
#이거랑 결과 동일
[x for x in numbers if odd(x)]
[x for x in numbers if x % 2 ]
```





3_`zip(*iterables)` 

* 복수의 iterable 객체를 모아(`zip()`)준다.

* 결과는 `튜플`의 모음으로 구성된 `zip object` 를 반환한다.

girls = ['jane', 'ashley', 'mary']
boys = ['justin', 'eric', 'david']

pair = list(zip(girls, boys))

print(pair)  -> 출력: [('jane', 'justin'), ('ashley', 'eric'), ('mary', 'david')]



