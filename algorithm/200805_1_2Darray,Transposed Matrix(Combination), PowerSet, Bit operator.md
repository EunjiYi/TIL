### 2차원배열 - 1차원 list를 묶어놓은 list.   배열의 배열이다.

- 배열: 같은 종류의 데이터가 여러개  ex. 숫자가 요소인 배열은 숫자가 여러개인 것.

- 이차원배열: 배열이 여러개/ 배열이 요소인 배열이다. = 근데, 각 요소가 배열이니까 또 반복문을 통해서 접근해야한다.

  >arr = [[0,1,2,3], [4,5,6,7]]
  >
  >arr의 길이는 2  #배열의 길이는 원소의 갯수, 2차원배열은 원소가 배열인 것.

  

순회 : 행 우선 순회, 열 우선 순회, 지그재그 순회(쫌 그렇대..ㅋㅋ)가 있는데 보통 `행/열 순회 2개`라고 생각해라

> 열 우선 순회는 변수로서 접근이 어렵다. 열 우선 순회는 정/직사각형 모양의 2차원배열에서만 가능하다. (열 우선 순회 쓰지마세요 ㅋㅋ)



중요한 건 전체 다 순회를 하면 된다! 는 것.

포트란에서 묵시적으로 정수는  i, j, k, l, m, n 이걸로 시작하라고 해서 아직까지도 i, j를 쓰고 있다.



파이썬은 직사각형말고 행마다 열 갯수가 다른 배열도 가능하다. 그런데 되도록 안쓴다. c는 array가 직사각형만 된다. 



상수: 변하지 않는 수 #보통 대문자로 쓴다. N, M, K 등

변수: 변하는 수



델타를 이용한 2차 배열 탐색

* 2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법

* 각각의 자리에서 4방향(상하좌우)로 가니까. 3중 for문

  ```python
  for x in range(len(ary)):
      for y in range(len(ary[x])):
          for mode in range(4):
              testX <- x + dx[mode]
              testY <- y + dy[mode]
              test(ary[testX][testY])
       
  # 인덱스가 벗어나는지 체크해야되고, 또 방문하면 안되니까 방문체크도 해야한다. 
  #            (-1,0)
  #              ^
  #              |
  #(0, -1) <- (0,0) -> (0,1)
  #			  |
  #    		  -
  #        	(1, 0)           
  ```

  



### 2차원 배열 입력받는 법

```python
2차원배열입력받는법.py 참고
내 깃헙 project_practice_python 레파지토리에 있음.
```





### 전치행렬

대각선으로 값 바꾸기

```python
for i in range(3):
    for j in range(3): #아니면 range(i+1, 3)으로 하고 if i < j 없앤다.
        if i < j:  #대각선 기준으로 절반만 한다.
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j] #swap
```





### 부분집합 = O(2^n)

파워셀, 멱집합 등등으로 불린다.

> 부분집합 생성법
>
> 집합의 원소가 n개일 때, 공집합을 포함한 부분집합의 수는 (2^n)개이다.
>
> 이는 각 원소를 부분집합에 포함시키거나 포함시키지 않는 2가지 경우를 모든 원소에 적용하면 됨!
>
> > 각 원소가 포함될지(1) 안될지(0)  => 그래서 (2^원소의개수) 만큼

 

부분집합 합 문제(Subset Sum)

유한 개의 정수로 이루어진 집합이 있을 때, 이 집합의 부분집합 중에서 그 집합의 원소를 모두 더한 값이 0 이 되는 경우가 있는지 알아내는 문제 = 완전검색

#### 우선 해당 집합의 모든 부분집합을 생성한 후에, 각 부분집합의 합을 계산해야한다!



-------

------

### 비트연산자

&,  | : 비트연산자

&&,  AND : 논리 연산자



비트연산자는 `양쪽의 피연산자를 비트로 바꾼다.` O & O 그래서 양쪽에 들어올 수 있는게 정수.

연산 결과도 정수로 뽑는다.  5 | 3 => 7

(내부적으로 2진수로 바꿔서 연산한다. 비트 연산을 쓰면 빠르다.)



#### shift << 연산자

`1 << n`: 2^n즉, 원소가 n개일 경우의 모든 부분집합의 수를 의미한다.

>1 << 3 (곱하기 2^3한 꼴)
>
>1 >> 3 ( 나누기 2^3 한 꼴)



#### 비트열 확인하는 것

`i & (1<<j)` : i의 j번째 비트가 1인지 아닌지를 리턴한다. - 이걸로 부분집합 쉽게 구할 수 있음.

> EX 3 = 011 = FTT



