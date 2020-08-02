200730 Data Structure_2



### Set

`mutable` `unorderd` `iterable`

set에 들어갈 요소들은 immutable한 걸로 들어가야 한다.

> 순서가 없다. 출력할 때마다 순서가 다른 결과가 나올 수 있다.
>
> 중복을 허용하지 않는다.
>
> 집합자체는 수정/추가 될 수 있지만 포함된 요소는 immutable해야한다.

```python
#1. 순서가 없다.
sample_set = {'a', 'b', 'c', 'd'}
print(sample_set)

#2. 중복을 허용하지 않는다.
sample_set2 = {1, 2, 3, 4, 5, 1, 2, 3}
print(sample_set2)

#3. 집합 자체는 수정/추가 될 수 있지만 포함된 요소는 immutable해야한다.
sample_set3 = {1, 'a', (1,3), [1,2]}
print(sample_set3)
# 이런 오류남 unhashable type: 'list'

print(sample_set[0])
# 'set' object is not subscriptable 오류남 -왜냐면 순서가 보장되지 않으니까 인덱스가 의미가 없다. 
```



어이없긴한데, 이뮤터블이랑 이터러블이랑 대충봐서 헷갈린 적이 있다.

조심



* .add(elem) - 세트에 elem 추가, 하나의 값만 가능

* .update(*others) - 여러개 추가 가능 _반드시 인자는 iterable해야한다._

  ```python
  a = {'사과', '바나나', '수박'}
  a.update({'토마토', '토마토', '딸기'}, {'포도', '레몬'})
  print(a)  # => {'바나나', '레몬', '딸기', '사과', '포도', '수박', '토마토'} 
  ```

* .remove(elem): 삭제, 삭제하려는 값 없으면 KeyError가 발생한다.

* .discard(elem): 삭제, 삭제하려는  키가 없어도 에러 발생 안한다. -> 프린트 찍어봐도 그냥 아무 변화 없음. 없는 값을 지우려 한거니까.

* .pop(): 임의의 원소를 제거해서 반환

  >  어떤 것이 팝으로 나올지 모름. 왜냐면 set은 순서를 보장하지 않기 때문에 어떤 것이 제거되고 반환될지는 모름





### 딕셔너리 - `Key: Value` 페어(pair)의 자료구조

`mutable` `iterable` `unorderd` (3.6부터는 순서가 보장이 되는데 메모리의 효율적 관리 차원에서다. 참고적으로 알아두기.)

```python
for dic in sample_dict:
    print(f'key: {dic}의 값은 {sample_dict[dic]}입니다.') # 딕셔너리의 키 값(dic)이 순회해서 나온다. 
    

# 나는 valud만 바로 반복해서 사용하고 싶다.
for dic in sample_dict.values():
    print(dic)   


    
# 나는 key와 value 동시에 반복해서 사용하고 싶다.    
for key, value in sample_dict.items():
    print(key)
    print(value)
    
for dic in sample_dict.items():
    print(dic) # 튜플로 값이 두 개가 넘어오기 때문에 #dic, value = (key, value)
    print(type(dic))
```



* .get(key[, default]) - [대괄호안에] 있는 건 옵션, 없어도 상관 없음, default는 기본적으로 None

  > 키를 통해 벨류 가져오고 KeyError  발생X, 키가 없으면 default값을 반환하는데 기본설정은 None이다. 
  >
  > 딕셔너리에서 .get(키) 굉장히 자주 많이 사용함.

* .pop(key[, default])

  > 키가 딕셔너리에 있으면 <u>제거하고 그 값을 반환.</u> 
  >
  > 키가 딕셔너리에 없으면 KeyError 발생 
  >
  > 값을 반환을 해주기 때문에 프린트로 찍어볼 수 있다. print(my_dict.pop('apple'))
  >
  > 두번째 인자로 default를 설정할 수 있다: my_dict.pop('melon', 0) # 멜론이라는 키가 없지만 디폴트 값이 0 이니까 0을 반환한다. 

* .update()

  > 값을 제공하는 key, value로 덮어쓴다. 
  >
  > ```python
  > my_dict = {'apple': '사과', 'banana': '바나나', 'melon': '멜론'}
  > 
  > # 기존의 값을 변경가능, 새로운 값 추가 가능
  > my_dict.update(apple='애플', pineapple = '파인애플') #중요한건, 키워드=벨류 꼴임! 인자 넣는 법임. `:`이거 아니다.
  > 
  > #제일 많이 하는 실수
  > #my_dict.update('apple' = '애플', 'pineapple' : '파인애플') 틀린거야!
  > print(my_dict)  #{'banana': '바나나', 'apple': '애플', 'pineapple': '파인애플'}
  > ```
  >
  > ```python
  > #키 값이 int라면?
  > #my_dict.update(1='one') -> keyword can't be an expression
  > #my_dict.update('1'='one') -> keyword can't be an expression
  > 
  > #이럴 땐 업데이트 사용하지 않고
  > my_dict[1] = 'one'
  > print(my_dict) #이렇게 접근해야한다. 
  > # 그런데 키 값으로 int 가능하긴 한데 잘 안한다. 모양 자체도 my_dict[1] 리스트랑 헷갈림
  > ```





## Dictionary comprehension

* 만드려는 값의 괄호를 먼저 씌워라. 리스트면 [] 딕셔너리면 cubic = {} 
* 그 다음 반복하려는 값을 넣어라 for num in numbers 
* 마지막으로 반복되는 값(키):벨류 num: num**2

```python
numbers = [1, 2, 3, 4, 5]

cub = {}
for num in numbers:
    cub[num] = num ** 2   #입력값 넣을 때 cub[num]이지, cub'['num']아니야.
    
print(cub)
```

```python
blood_types = {'A': 40, 'B': 11, 'AB': 4, 'O': 45}
negative_blood_types = {'-'+key: blood_types[key] for key in blood_types}
print(negative_blood_types)  #{'-A': 40, '-B': 11, '-AB': 4, '-O': 45}
```



list comprehension과 동일하게 조건문 추가 가능.

생소하니까 예시 하나씩 따라해봤다. dusts = {'서울': 72, '대전': 82, '구미': 29, '광주': 45, '중국': 200}



미세먼지 농도가 80초과하는 지역만 뽑아라

bad_air_city = {key: value for key, value in dusts.items() if value>80}



미세먼지 농도가 80초과는 나쁨, 80이하는 보통으로 하는 value를 가지는 bad_air_city를 만들어라.

bad_air_city = {key: '나쁨' if value > 80 else '보통' for key, value in dusts.items()}



elif 사용가능 - 그런데 elif 형태로 쓰면 안되고 if else 열거해야함  ->  이것도 list comprehension과 동일

bad_air_city = {key: '매우나쁨' if value >150 else '나쁨' if value>80 else '보통' if value > 30 else '좋음' for key, value in dusts.items()}



직접 해보니까 별거아니네~(허세충전)