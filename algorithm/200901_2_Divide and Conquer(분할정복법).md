## **Divide and Conquer**

유래: 1805.12.02 나폴레옹의 전략

아우스터리츠 전투에서 나폴레옹이 연합군의 중앙부로 쳐들어가 연합군을 둘로 나누고, 둘로 나뉜 연합군을 한 부분씩 격파한 전략



### 설계 전략

* 분할(Divide): 해결할 문제를 여러 개의 작은 부분으로 나눈다.
* 정복(Conquer): 나눈 작은 문제를 각각 해결한다.
* 통합(Combine): (필요하다면) 각각 해결된 해답을 모은다.



### 분할정복의 대표적인 예제 = 거듭제곱

O(n)

```python
def Power(Base, Exponent):
    if Base == 0:
        return 1
    
    result = 1 # Base^0 = 1이니까. 0승은 1
    for i in range(Exponent):
        result *= Base
    return result
```



O(lon2n)

Base가 짝수일 때, 홀수일 때 나누기

짝수일 때: C^n = C ^(n/2) * C^(n/2)

홀수일 때: C^n = C^((n-1)/2) * C^((n-1)/2) * C

```python
def Power(Base, Exponent):
    if Exponent == 0 or Base == 0:
        return 1
    
    if Exponent % 2 == 0:
        NewBase = Power(Base, Exponent/2)
        return NewBase * NewBase
    else:
        NeWBase = Power(Base, (Exponent-1)/2)
        return NewBase * NewBase * Base
```



