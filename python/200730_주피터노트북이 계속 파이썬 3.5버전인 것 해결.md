200730 주피터노트북이 계속 파이썬 3.5버전인 것 해결



#### 오늘의 트러블

주피터노트북 파이썬버전 3.5나오는 것 해결(서버는 정상적으로 3.7)



#### 현상

윈도우 cmd에서 `python -V`찍었을 때 3.7.7 나오는데 `pip list`하면 주피터가 없음 _읭?_

윈도우 환경변수 Path에서 python35를 맨 위로 놓고 python --verson찍어서 3.5.3 나오는거 확인한 뒤, pip list하면 주피터가 깔려있음 

--> 즉, 3.5커널에 주피터 노트북이 깔림!!!



#### 조치

충돌날 수 있으니(경로 혼동이 있을 수 있다.) 환경변수 Path에서 python35을 더 우선으로 두고, 3.5버전에서 	pip uninstall jupyterlab` 하고 나서 

환경변수 Path에서 python37을 더 우선순위로 바꾸고, 3.7 버전에서` pip install jupyterlab`하니까 끝!
_그동안 주피터가 파이썬3.5커널을 보고 있었음...그래서 계속 3.5 쓰고 있었음_



#### 추가

주피터에서 파이썬버전 확인하는 코드
`import sys`

`print(sys.version)`

\+ 그러면서 추가로 알게 된 사항

>  파이썬 버전별로 (3.5 3.7) 가비지콜렉터가 하는 일이 조금 다름
>
> initdel.py파일만 봐도 3.5버전에서는 current가 쌓이기 전에 바로 소멸시켜버려서 current : 0나오는데,
>
> 3.7버전에서는 current가 일단은 3으로 찍히고 그 다음에 소멸시킴



```python
#initdel.py

# class Myclass:
#     # 생성자
#     def __init__(self,value):
#         self.value = value
#         print(f'{self.value}가 생성되었습니다.')

#     # 소멸자
#     def __del__(self):
#         print('소멸되었습니다.')

# m1 = Myclass(1)


class Doggy:
    num_of_dogs = 0
    birth_of_dogs = 0
    
    def __init__(self, name, breed, age=1):
        self.name = name
        self.breed = breed
        self.age = age
        Doggy.num_of_dogs += 1
        Doggy.birth_of_dogs += 1
    
    def __del__(self):
        Doggy.num_of_dogs -= 1
        
    def bark(self):
        return '왈왈!'
        
    @classmethod
    def get_status(cls):
        #return f'Birth: {cls.birth_of_dogs}, Current: {cls.num_of_dogs}'
        print('Birth: %s' %cls.birth_of_dogs, end = ' ')
        print('Current: %s' %cls.num_of_dogs)

d1 = Doggy('초코', '푸들')
d2 = Doggy('꽁이', '말티즈')
d3 = Doggy('별이', '시츄')
d1 = Doggy('초코', '푸들')
d2 = Doggy('꽁이', '말티즈')
d3 = Doggy('별이', '시츄')

print(d1.bark())
Doggy.get_status()
```

출력: 

왈왈!
Birth: 6 **Current: 3**