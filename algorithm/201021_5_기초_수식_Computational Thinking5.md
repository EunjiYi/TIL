## 기초 수식

#### 약간의 설명

`알고리즘의 시간복잡도`를 표현할 수 있는 다양한 수식들이 존재한다.

풀이법을 익혀 두어야 알고리즘의 시간복잡도를 계산할 수 있고, 알고리즘이 시간이 얼마나 걸릴지 예측할 수 있다. 



### 연습문제: 다음 재귀식들을 O() notation 수준으로 풀어라. 

재귀식은 점화식이 아니다.

무슨 뜻이냐면, T(n) = n개 만큼 들어왔을 때 연산량

T(n) = T(n-1) + 1의 뜻 : n개 만큼 들어왔을 때의 연산량이, n-1개만큼 들어왔을 때의 연산량보다 하나 많다. 

#### 문제1

![image-20201022133859372](images/image039.png) 

문제 

(n=0일 때) T(0) = 1

(n != 0일 때) T(n) = T(n-1) + 1



 풀이

T(n) = T(n-1) + 1

T(n-2) + 1 +1 

= T(n-k) + k

=T(0) + n

= 1 + n

-> O(n)



#### 문제2![image-20201022134035767](images/image041.png)



#### 문제3

![image-20201022133947502](images/image040.png)

![KakaoTalk_20201022_190813405](images/image058.png)



#### 문제4 ![image-20201022134107001](images/image042.png)

![KakaoTalk_20201022_191308287](images/image057.png)



#### 문제5 ![image-20201022134135815](images/image044.png)

![KakaoTalk_20201022_192349076](images/image056.png)



#### 문제6![image-20201022134156992](images/image045.png)



#### 문제7![image-20201022134220223](images/image046.png)

![KakaoTalk_20201023_102641143](images/image037.png)



![KakaoTalk_20201023_102701333](images/image038.png)



#### 문제8![image-20201022134248247](images/image047.png)

#### 방법1 = 조화급수의 합으로 증명

![KakaoTalk_20201023_105403048](images/image071.png)

#### 방법2 = 수 비교

![KakaoTalk_20201023_110045940](images/image072.png)



#### 문제9![image-20201022134311199](images/image048.png)



#### 문제10![image-20201022134330945](images/image049.png)
