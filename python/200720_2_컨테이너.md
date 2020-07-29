200720_data_container





`시퀀스`: 데이터가 순서대로 나열된(ordered) 형식, 그렇다고 `정렬되었다(sorted)`는 NONO

1. 순서를 가질 수 있다.
2. **특정 위치의 데이터를 가리킬 수 있다.**

리스트(list), 튜플(tuple), 레인지(range), *문자형(string)*, *바이너리(binary)* 

---------



` 비 시퀀스형`(Non-sequence) 컨테이너

셋(set), 딕셔너리(dictionary)

3. set
   순서가 없는 자료구조(수학에서 집합과 동일)
   중괄호{}로 만들고, 순서가 없고 중복된 값이 없다. ->그래서 리스트에 중복 제거할 때 쓸 수 있다.

* 아 이거 헷갈리는데
빈 집합을 만들려면 set()을 사용해야함 ({}로 사용 불가능.)



4. dictionary

- `{}`로 만들거나 `dict()`

- `key`는 **변경 불가능(immutable)한 데이터**만 가능. 

  ```
  immutable : 
  숫자, 글자, 참거짓, string, integer, float, boolean, tuple, range,() frozenset()
  
  mutable: 
  set, dictionary, list, 사용자가 만든 데이터 타입
  ```

- `value`는 `list`, `dictionary`를 포함한 모든 것이 가능

- items() 메소드를 쓰면 key, value 확인 가능

------------



1. 리스트 만들기

l = []
ll = list()



2. 튜플

리스트와 유사하지만, `()`로 표현

수정 불가능(불변, immutable), 읽을 수 밖에 없다. (직접 사용하기 보다는 파이썬 내부에서 다양한 용도로 활용됨)

X, Y = 1, 2 이렇게 정의할 때나 변수의 값 swap할 때, 파이썬 내부에서 튜플로 처리됨

> 
>
> 주의사항 : 
>
> 하나의 항목으로 구성된 튜플은 값 뒤에 쉼표를 붙여서 만든다. 이때 len찍어보면 1이다!

```python
single_tuple = ('hello',)
print(type(single_tuple)) -> <class 'tuple'>
print(len(single_tuple)) -> 1
```



__(이걸 왜 자꾸 까먹는지 모르겠는데 기억 좀 하자)__

s * n	: n번만큼 반복하여 더하기

s.count(x)	x의 개수

ex 

```python
[0] * 6
결과 [0, 0, 0, 0, 0, 0]
```





------------



str tuple set 이것만 list int형변환가능

```python
li = {1,2,3,4}
print(str(li)) ->ok
print(tuple(li)) -> ok
print(dic(li)) ->오류남 'cannot convert dictionary update sequence element #0 to a sequence'
```

