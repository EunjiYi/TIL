0727   복습

###  예외처리

* 문법 에러: 문법적 오류가 있을 때

* exception
  - NameError
  - TypeError
  - ValueError: int('3.5')
  - ZeroDivisionError
  - IndexError
  - KeyError
  - ModuleError
  - ImportError # 임포트시 오타 났을 때 주로 남

 

* 예외처리

  * try & except

    * ```python
      try:
          <코드블럭 1>
      except 예외1:
          <코드블럭 2>
      except 예외2. 예외3:
          <코드블럭 3>
      except 예외4 as err:
          print(f'{err}로 에러메시지를 프린트 할 수 있다.')
      else: #코드블럭1에서 에러가 발생하지 않았을 때만 실행된다.
      	<코드블럭 4>
      finally: #에러 발생 유무와 상관없이 반드시 실행
          <코드블럭 5>
      ```



* 예외 발생
  *  raise: 예외를 강제로 발생 range valueError('숫자를 입력해주세요. ')
  *  assert:  예외를 강제로 발생
    * 상태를 주로 검증하기 위해서: assert Boolean expression, error message
    * assert type(x) == int, '숫자들이 아닙니다.'





### 데이터구조 1

	* 데이터 구조: 데이터에 편리하게 접근을 하고, 변경하기 위해 데이터를 저장하거나 조작하는 방법
 * 순서가 있는 구조(ordered)
   	*   문자열, 리스트
 * 순서가 없는 구조(unordered)
   	* set, dictionary



#### 	문자열

  - 변경할 수 없다. (immutable) / 순서가 있다.(ordered) / 순회 가능하다(iteratble)

  - String method  문자열을 조작하기 위한 메소드들

      - 값을 변경하는  method

          - replace(바꿀 문자열(old), 바꾸려는 문자열(new)[, count])

            ```python
            'yay!'.replace('a', '-')  #'y_y!'
            'wooooooo'.replace('o', '!'. 3) #w!!!oooo 바꾸려는 수 지정
            
            ```

        

          - strip([char])

              - 특정 문자를 지정하면 해당 문자를 양쪽에서 찾아 제거한다.
              -  lstrip 해당 문자를 왼쪽에서 찾아서 제거 / rstrip 오른쪽에서 찾아서 제거

          - split([char]) *많이 사용

              - 문자열을 특정 단위로 나누어서 리스트로 반환

                ```
                input = '1 2 3 4 5'
                li_input = input.split() #['1', '2', '3', '4', '5']
                ```

                ```
                a = 'a_b_c'
                print(a.split('_')) #['a', 'b', 'c']
                print(a.split('b')) #['a_', '_c']
                ```

                

          - join(iterable) *많이 사용

              - 특정 문자열로 만들어서 반환

              - ```python
                word = '배고파'
                a = '!'.join(word) #'배!고!파'
                words = ['a', 'b', 'c']
                b= '_'.join(words) #'a_b_c'
                ```

      - 문자변경

          - capitalize()
          - title() : New York Times
          - upper()
          - lower()
          - swapCase()

      - 문자열 관련 검증

          -  istitle() #타이틀인지 검증 후 T/F 반환

          - isalpha() #해당 문자가 알파벳으로 구성되어 있는지

          - isspace() #공백이 있는지

          - isupper() / islower() #대문자인지, 소문자인지

          - isdecimal() # 순수 int로 형변환이 가능한 문자열인지 확인

          - isdigit() #윗첨자도 숫자로 인식

          - isnumeric() # 분수의 특수기호, 특수로마자도 숫자로 인식

            ​	주의: 해당  decimal, digit, numeric은, float 형태의 문자열을 다 False 로 반환한다. 

            ​			타입 확인이 필요하다.

 - 기타 문자열 관련 메소드

   - dir('string')이라고 하면 관련된 메소드들 확인이 가능하다.

   

### 리스트

  - 변경 가능(mutable), 순서가 있고(ordered), 순회가능(iterable)하다.

  - List Method

      - 값을 추가 및 삭제

          - append(x)

          - extend(iterable)

          - insert(i,x)

            

          - remove(x) #x가 리스트에 많이 있어도 처음만난 x만 삭제한다. 지우려는 값이 없으면 에러 발생한다.

          - pop() : 정해진 위치에 있는 값을 삭제하고 그 값을 반환. i에 값이 없으면 마지막 항목을 삭제하고 반환을 한다.

          -  clear(): 리스트에 있는 모든 요소 싸그리 삭제

            

    - 탐색 및 정렬

      - index(x) : 가장 처음으로 찾는 x 값을 찾아서 인덱스를 반환한다. 값이 없으면 에러 발생

      - count(x): 리스트에서 x의 갯수를 count 후 반환

        

        ------- sort()랑 sorted() 차이 꼭 알기 ---------

      - sort(): None 을 반환, 리스트를 정렬, 이때 원본을 변경한다.**사용주의** `sort(reverse=False)`//이렇게 하면 내림차순이 된다.True면 오름차순 = default

        -  sorted(iterable) #이건 리스트의 메소드(함수)가 아니라 파이썬 내장함수

          ```python
          정렬된 값을 반환, 그리고 원본은 유지를 한다.
          문자열.sort()를 못하니까 문자열 정렬시에는 sorted()사용한다.
          sorted(iterable, reverse = False) 가능. 디폴트는 오름차순 트루
          ```

        

      - reverse(): 정렬 없이 앞뒤를 뒤집는다.

        * reversed(): 앞뒤를 뒤집어 준다. `list_reverseiterator object  반환`



 * 리스트복사

    * ```python
      a = {1,2,3}
      b =	a
      b[0] = 10
      print(a) #{10, 2, 3}
      ```

    * slicing 을 활용하여 복사

      ```python
      a = [[1,2], [3,4]]
      b = a[:]
      
      a[0] = [4, 5]
      print(b)
      ```

      

    * list의 메소드를 활용해서 복사

      ```python
      a = [1, 2, 3]
      b = list(a)
      
      b[1] = 200
      print(a)
      ```

      

   - copy 모듈을 활용

     ```python
     import copy
     
     a = [1,2,3]
     b = copy.copy(a)
     ```

   - deepcopy

     ``` python
     import copy
     
     a = [1, 2, [1, 2]]
     b = copy.deepcopy(a)
     ```



   * 데이터 분류

        *	 immutable
          			*	number, string, bool, range, tuple, frozenset

        *	mutable
          			*	list, set, dictionary



-----------

  * Built-in Function

      * map(function, iterable)

        		* iterable 한 데이터를 인자로 받아서 모든 요소에 function 을 적용한 후 결과를 map object로 반환		

        ```python
        def sqaure(num):
            return num ** 2
        	
        numbers = [1,2,3,4,5]
        double_li = list(map(square, numbers))
        print(double_li)
        
        ```

        ```python
        input = '1 2 3 4 5'
        numbers = input.split()
        new_numbers = list(map(int, numbers))
        ```

    * filter(function, iterable)

      * function 의  return 값이 True인 값만 추출. `filter object`값을 반환

        ```python
        def pos_num(num):
            if num > 0:
            	return num
            else:
                return False
            
        numbers = list(range(-10. 10))
        pos = list(filter(pos_num, members)) #[1, 2, 3, 4, 5, 6, 7, 8, 9]
        
        ```

    * zip(*iterable)

      * 복수의 iterable한 객체를 모아준다. tuple 모음으로 구성된 zip object를 반환.

        ```python
        girls = ['jane', 'iu', 'mary']
        boys = ['justin', 'david', 'kim']
        ranking = [1, 2, 3]
        
        couples = list(zip(girls, boys)) 
        #[('jane', 'justin', 1), ('iu', 'david',2), ('mary','kim',3 )]
        #zip은 길이가 같은 것끼리 사용하길 추천하고, 길이가 다르면 길이가 짧은 것을 기준으로 묶어준다.
        ```

        * 길이가 다르다면 짦은 객체를 기준으로 합쳐주고 나머지는 무시된다.

        * 되도록이면 길이가 같은 객체를 사용하는 것이 좋다.

        * itertools 내장모듈 안에 zip_longest 함수를 사용하면 긴 것을 기준으로 합쳐준다.

          ```python
          (사용할 일이 드물다, 거의 없음)
          from itertools import zip_longest
          num1 = [1,2]
          num2 = [4,5,6]
          list(zip(num1, num2)) #[(1.4), (2,5)]
          zip_longest(num1, num2, fillvalue=0) #비어있는 값을 뭘로 채울 것이냐 ->0으로 채우겠다.
          # [(1,4), (2,5), (0,6)]
          ```





### 데이터구조2

### 세트(set)

	* 변경가능하고, 순서가 없고, 순회는 가능
	* 집합의 요소는 유니크 하다. 중복이 불가능
	* 집합의 요소는 immutable한 값만 가능. mutable 객체를 넣으면 TypeError 발생
 * Set Methods
    * 추가 및 삭제
      	* add(elem): 값을 하나 추가시킬 때 사용한다.
      	* update(*others): 여러 개의 값을 넣을 때(값은 immutable한 것을 넣어줘야한다.)
      	* remove(elem): 값을 삭제하고 만약 값이 없으면 KeyError 발생
      	* discard(elem): 값을 삭제를 하고 만약 값이 없으면 에러발생하지 않는다.
      	* pop(): 임의의 요소를 제거한 후 반환해준다.



### 딕셔너리

* 변경가능하고, 순서가 없고, 순회가능하다. (값을 키로 찾기 때문에 순서를 신경쓸 필요가 없다.)

* dictionary Methods

  * get(key[, default])  default=None

    * key를 통해서 해당 value를 가져온다.

       dic['key']: 키 값을 직접 넣어서 값을 가져올 때 키가 없으면 KeyError  발생

    * key가 없어도 에러를 발생하지 않음. None 을 반환

  * pop(key[, default])

    * key가 있으면 dictionary에서 제거하고, key가 없으면 default값을 반환
    * default가 없으면 keyerror 발생. (get이랑 헷갈리지말고 오류나기 싫으면 디폴트를 꼭 설정하자.)

  * update()
    * 1개 이상의 값을 ` key = 'value'` 의 형식으로 값을 추가할 수 있다.
    * key가 존재하면 그 값을 수정
    * key가 존재하지 않으면 새롭게 추가
  * keys()
    * 해당 dictionary의 키를 리스트로 반환 (`dict_key_object`)
  * values()
    * 해당 dictionary의 value를 리스트로 반환(`dict_value_object`)
  * items()
    * 해당 dictionary의 key와 value를 tutple 형태로 반환.(`dict_items_object`)

* Dictionary 순회

  ```python
  # 1. dictionary를 for로 순회했을 때
  for dic in dicts:
      print(dic) #dicts의 키값이 들어있다.
      
  # 2. keys로 순회했을 때
  for dic in dicts.keys():
      print(dic) # dicts 의 키 값이 들어있다.
      
  # 3. value로 순회했을 때
  for val in dicts.values():
      print(val) #dicts의 value만 들어있다.
      
  #4. items로 순회했을 때
  for dic in dics.items():
      print(dic) #dic 에는 (key, value) 형태의 tuple 값이 들어있다.
      
  for key, value in dicts.items():
      print(key)
      print(value)
  ```



## #List Comprehension  리스트단축

* 간결함

* pythonic 한 코드

* 속도도 빠른 편

* 무분별하게 사용하게 되면 코드의 가독성이 떨어질 수 있다. 

* ```python
  # 기본 형태
  li_comp = [식 for 변수 in iterable]
  li_comp2 = list(식 for 임시변수 in iterable)
  
  # 기본 형태 + 조건식
  li_comp3 = [ 식 for 임시변수 in iterable if 조건식]
  li_comp4 = [ 식1 if 조건식 else 식2 for 임시변수 in iterable]
  
  li_comp5 = [ 식1 if 조건식 else `if 조건식2 else 식3` for 임시변수 in iterable]
  # 식이 있는 자리에 계속 조건식추가해서 넣을 수 있다.
  ```

  