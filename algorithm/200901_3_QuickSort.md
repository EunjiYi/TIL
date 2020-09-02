## Quick Sort

퀵 정렬

- 주어진 배열을 두 개로 분할하고 각각을 정렬하는데, 합병정렬이 그냥 두 부분으로 나누는 반면에 퀵정렬은 분할 할 때 기준아이템(pivot item)을 중심으로 이보다 작은 것은 왼쪽/ 큰 것은 오른쪽에 위치시킨다.
- 각 부분정렬이 끝나면 합병정렬은 말그대로 '합병'이 필요한데, 퀵정렬은 다른 후처리 작업이 필요없다.



퀵정렬의 최악의 시간복잡도는 O(n^2)으로, 합병정렬에 비해 좋지 못하다.

그런데 빠른 정렬이라 부르는 이유는 평균복잡도가 O(nlogn)이기 때문이다. = 평균적으로 가장 빠름



```python
def quickSort(a, begin, end):
    if begin < end:
        p = partiion(a, begin, end)
        quickSort(a, begin, p-1)
        quickSort(a, p+1, end)
        
def partition(a, begin, end):
    pivot = (begin + end) // 2
    L = begin
    R = end
    while L < R:
        while(a[L] < a[pivot] and L<R): 
            L += 1
        while(a[R] >= a[pivot] and L<R): 
            R -= 1
		if L < R:
            if L == pivot: 
                pivot = R
            a[L], a[R] = a[R], a[L]
	a[pivot], a[R] = a[R], a[pivot]
    return R
```



앞쪽에는 피벗보다 작은값, 뒤쪽에는 피벗보다 큰 값

> L : 오른쪽으로 이동하면서 피벗보다 큰 값을 찾는다.
>
> R : 왼쪽으로 이동하면서 피벗보다 작은 값을 찾는다.



`피벗은 어느 자리여도 상관없다. `정렬되어있지 않은 상태에서 특정 값 하나를 잡는 것 뿐이다.

(물론 피벗의 위치에 따라 최대 3배까지 효율이 차이난다고는 함)

> 구현할 때 맨 앞 값을 피벗으로 잡으면 아주살짝 쉽기 때문에 맨 앞을 피벗으로 하겠다.!



```python
#퀵정렬 구현 코드


```



