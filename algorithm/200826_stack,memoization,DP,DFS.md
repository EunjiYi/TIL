### 스택의 push 알고리즘

```python
def push(item):
    s.append(item)
```



### 스택의 pop 알고리즘

```python
def pop():
    if len(s) == 0:
        #underflow
        return
    else:
        return s.pop(-1)
```



### Function Call (함수호출)

프로그램에서의 함수 호출과 복귀에 따른 수행순서를 관리

- 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 `후입선출`구조이므로, 후입선출 구조의 스택을 이용하여 수행순서를 관리한다.
- 함수 호출이 발생하면 호출한 함수 수행에 필요한 지역변수, 매개변수 및 수행 후 복귀할 주소 등의 정보를 스택 프레임(stck frame)에 저장하여 시스템스택에 삽입한다.
- 함수의 실행이 끝나면 시스템 스택의 top원소(스택 프레임)를 삭제(pop)하면서 프레임에 저장되어있던 복귀주소를 확인하고 복귀
- 함수호출과 복귀에 따라 이 과정을 반복하여 전체 프로그램 수행이 종료되면 시스템 스택은 공백 스텍이 된다.



-> 스텍에 쌓아서 리턴을 한다.!



재귀호출 등에서의 중복호출 보완하기: 메모이제이션

### 메모이제이션 Memoization

이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록한다.

전체적인 실행속도를 빠르게 하는 기술이다.

동적계획법의 핵심이 되는 기술이다.

> 참고:
>
> memoization: to put in memory 이란 의미



피보나치 수를 구할 때 fibe(n)의 값을 계산하자마자 저장하면(memoize) 실행시간을 O(n)으로 줄일 수 있다.



### Memoization 방법을 적용한 알고리즘

```
memo를 위한 배열을 할당하고, 모두 0으로 초기화한다.
memo[0]을 0으로 memo[1]는 1로 초기화한다.

def fibo1(n):
	global memo
	if n >= 2 and len(memo) <= n :
		memo.append(fibo(n-1) + fibo1(n-2))
	return memo[n]
	
memo = [0, 1]
```



### 동적계획법(Dynamic Programming)

그리디 알고리즘처럼 `최적화 문제` 

> 먼저 입력 크기가 `작은 부분문제들을 모두 해결한 후에 그 해들을 이용하여 보다 큰 크기의 부분문제들을 해결`하여서 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘이다.



#### 피보나치 수 DP적용

> 피보나치수는 최적부분구조로 이루어져있다. (작은 문제의 답으로 큰 문제의 답을 얻으니까)
>
> 1. 문제를 부분문제로 분할한다.
> 2. 가장 작은 부분문제부터 해를 구한다. (bottom up)
> 3. 그 결과를 테이블에 저장하고 테이블에 저장된 부분문제의 해를 이용해서 상위 문제의 해를 구한다.
> 4. 점화식 f[i] = f[i-1] + f[i-2]
>
> ```python
> #반복문으로 되어 있다. 
> 
> def fibo2(n):
>     f = [0, 1]
>     
>     for i in range(2, n + 1):
>         f.append(f[i-1] + f[i-2])
>         
>    	return f[n]
> ```





### DP의 구현 방식 -> DP의 핵심기술 메모이제이션

* recursive 방식: fibo2()  #재귀적DP
* iterative 방식: fibo3()  #반복적DP



memoization을 재귀적 구조에 사용하는 것보다 `반복적 구조`로 DP를 구현하는 것이 성능면에서 더 효율적

>  재귀적 구조는 내부 시스템호출 스텍을 사용하는, `오버헤드가 발생`하기 때문이다.



----------



### DFS(깊이우선탐색)

> 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이우선탐색을 반복해야하므로 후입선출구조의 스택을 사용한다.



DFS 알고리즘 - 재귀

```
DFS_Recursive(G, v)
	visited[v] <- TRUE  // v 방문설정
	
	FOR each all w in adjacency(G, v)
		IF visitied[w] != TRUE
        	DFS_Recursive(G, w)
```

DFS를 재귀로 돌리면 제일 처음 자리로 돌아온다. 스텍을 다 비워야하니까.



DFS 알고리즘 - 반복

```
STACK s
visited[ ]
DFS(v)
	push(s,v)
	WHILE NOT isEmpty( s )
		v <- pop(s)
		IF NOT visited[v]
			visit(v)
			FOR each w in adjacency(v)
				IF NOT visited[w]
					push(s, w)
```

스텍을 이용

