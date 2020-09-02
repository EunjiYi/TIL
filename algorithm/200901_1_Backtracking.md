## 백트래킹



모든 후보를 검사하지 않는다.

* 어떤 노드의 유망성을 점검한 후에 유망(promising)하지 않다고 결정되면 그 노드의 부모로 되돌아가서 (backtracking) 다음 자식 노드로 간다.
* 어떤 노드를 방문하였을 때 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 한다.
* 가지치기(Pruning): 유망하지 않는 노드가 포함되는 경로는 더이상 고려하지 않는다.

  > 가지치기란
  >
  > `각 노드에서 유망하지 않은 노드들은 탐색하지 않는 것`을 **가지치기 Pruning** 라 한다.



Backtracking 기법은 해를 찾는 도중에 `막히면` (즉, 해가 아니면) 되돌아가서 다시 해를 찾는 기법이다.

백트래킹 기법은 최적화(Optimization) 문제와 결정(Decision) 문제를 해결할 수 있다.

결정문제: 문제의 조건을 만족하는 해가 존재하는지 여부를 yes, no로 답하는 문제

> 미로찾기
>
> n-Queen 문제
>
> Map coloring (이웃한 구역에 같은 색 안 칠하는 것)
>
> 부분집합의 합(Subset Sum)문제 
>
> 등



### 백트래킹과 깊이우선탐색과의 차이

* 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 더이상 그 경로를 따라가지 않음으로써 시도의 횟수를 줄인다. (Prunning 가지치기)

* 깊이우선탐색이 모든 경로를 추적하는데 비해 `백트래킹은 불필요한 경로를 초기에 차단한다.`

* 깊이우선탐색을 가하기에는 경우의 수가 너무나 많다. 즉, N!가지의 경우의 수를 가진 문제에 대해 깊이우선탐색을 가하면 당연히 처리불가능한 경우가 생긴다. (대략 10!를 10초에 처리한다고 함(그래서 n=10까지는 완전검색으로 가능하긴 함...ㅎ) `순열 nPn = n!`)

  > n- queen 문제에서 n을 10으로 두면 조금 걸린 뒤에 답 출력됨

* 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만, 이 역시 최악의 경우에는 여전히 `지수함수의 시간(Exponential Time)`을 요하므로 처리가 불가능하다.

> n-queen 문제에서
>
> 순수한 깊이우선탐색 = 155노드
>
> 백트래킹 = 27노트





### 일반 백트래킹 알고리즘

1. 상태공간트리의 깊이우선탐색을 실시한다.
2. 각 노드가 유망한지 점검한다.
3. 만약 그 노드가 유망하지 않아면, 그 노드의 부모노드로 돌아가서 검색을 계속한다.

```python
def checknode(v): #node
if promising(v):   #실제로 구현할 때는 if not promising: return 이런식으로 조건문 짠다.
    if there is a solution at v:
        write the solution
    else:
        for u in each child of v:
            checknode(u)
```





`+ review`

### 재귀호출을 이용한 부분집합 생성 알고리즘

백트래킹 요소 안 들어간 것.

```python
A[] : 해당원소의 포함여부를 저장(0,1)
def powerset(n, k): # n개의 원소 중 K개를 가진 부분집합 구하기.
    if n == k:
        PRINT
    else:
        A[k] = 1
        powerset(n, k + 1)
        A[k] = 0
        powerset(n, k + 1)
```





`백트래킹 =  재귀호출 + 가지치기`

### 백트래킹을 활용한 부분집합 구하기

n개의 원소가 들어있는 집합의 2^n개의 부분집합 만들기: 

백트래킹에서는 true (1)또는 false (2)값을 가지는 항목들로 구성된 n개의 배열을 만든다. 

> 해당 배열의 i번째 항목은, i번째 원소가 부분집합의 값으로 들어가는지 아닌지를 나타낸다.



#### powerset을 구하는 백트래킹 알고리즘 

```python
def backtrack(a, k, input):
    global MAXCANDIDATES
    c = [0] * MAXCANDIDATES
    
    if k == input: #찾던 답이면 
        process_solution(a, k) # 원하는 작업을 한다.
    else:
        k += 1
        ncandidates = constrcun_candidates(a, k, input, c)
        for i in range(ncandidates):
            a[k] = c[i]
            backtrack(a, k, input)
            
def construct_candidates(a, k, input, c):  # 사실 여기서는 1(True), 0(False) 두 개라 이렇게 함수로 따로 뺄 필요는 없는데, 후보군을 둔다는 의미를 남기고 싶어서 만듬. 나중에 다른 코드에서 후보군이 2개가 아니라 많아져도 괜찮도록.
    c[0] = True
    c[1] = False
    return 2

MAXCANDIDATES = 100
NMAX = 100
a = [0] * NMAX
backtrack(a, 0, 3)
```





### 백트래킹을 이용하여 순열구하기

> 접근방법은 부분집합 구하기와 유사하다.

```python
def backtrack(a, k, input):
    global MAXCANDIDATES
    c = [0] * MAXCANDIDATES
    
    if k == input:  #찾던 답이면
        for i in range(1, k+1): #원하는 작업을 한다.
            print(a[i], end = " ")
        print()
    else:
        k += 1
        ncandidates = construct_candidates(a, k, input, c)
        for i in range(ncandidates):
            a[k] = c[i]
            backtrack(a, k, input)
            
def construct_candidates(a, k, input, c):
    # in_perm: 순열에 이미 포함되어 있는지 표시하는 배열
    in_perm = [False] * NMAX
    
    for i in range(1, k): 
        in_perm[a[i]] = True  #내가 만들고 있는 순열에 이미 포함된 요소라면 True 표시를 해라.
    
    ncandidates = 0
    for i range(1, input + 1):
        if in_perm[i] == False:  #이건 반대로 순열에 포함되지 않은 요소
            c[ncandidates] = i
            ncandidates += 1
     return ncandidates
```



#### 조합적 문제

1. 부분집합

2. 순열, 중복순열

3. 조합, 중복조합

   셋 다 재귀로,

   여기에 + 가지치기를 하면 백트래킹

   `(부분집합의 순열 -> 부분집합 만들어서 순열) or (부분집합 dfs돌린다) 등`



### 재귀 호출을 통한 순열 생성

```python
perm(n, k)
	IF k == n
		print array #원하는 작업 수행
	ELSE
		FOR i in k -> n-1
			swap(k, i)
			perm(n, k+1)
			swap(k, i)
```

재귀도 스택을 사용하기 때문에 DFS



```
순열짜는 방법 3가지
- swap(바로 위 코드)
- visited 이용(비트/재귀)
- 백트래킹(바로 위위 코드)
```



#### visited이용 예시

```c++
//재귀호출을 이용한 BF 구현

//getResult: result[]에 답을 채워 나가면서 출력하는 함수
//현재 current을 채울 차례이며, 총 n개의 숫자를 채워야하는 상황

void getResult(int current, int n, int result[]){
    if(current >= n){
        print(result);
    }
    else{
        for(int i=1; i<-n; i++){
            if(isNotContaining(result, i)){
                result[current] = i;
                getResult(current+1, n, result);
                result[current] = 0;
            }
        }
    }
}
```

