200721_control_flow

주피터노트북 설치cmd창에pip install jupyterlab

```
주피터 노트북 설치

그냥 작업표시줄에서 git bash열기

python --version
pip --version
pip install jupyter
pip list | grep notebook
pip list | grep jupyter

cd
pwd 
그리고 기동
EUNJI YI@LAPTOP-ON7NAJRQ MINGW64 ~/python
$ jupyter notebook
jupyter notebook
```

```
$jupyter notebook --generate-config
해서 config 파일 생성하고 경로를 크롬으로 바꾸면 되는데,
이렇게 하지 말고 간단하게 윈도우 기본 앱을 크롬으로 바꿔서 해결함.
```





쉬운데 은근 헷갈림 예제 많이 적어놔야지

조건 표현식(Conditional Expression) = 삼항 연산자(Ternary Operator)

**true_value if <조건식> else false_value**

```
print('0 보다 큼') if num > 0 else print('0 보다 크지않음')
value = num if num >= 0 else -num
result = '홀수입니다.' if num % 2 else '짝수입니다.'
```



`for` 문은 시퀀스(String, Tuple, List, Range)나 다른 순회가능한 객체(iterable)의 요소들을 순회



### `for-else` - 반복문을 끝까지 다 시행한 이후에 else 실행

- 반복에서 리스트의 소진이나 (`for` 의 경우) 

  조건이 거짓이 돼서 (`while` 의 경우) 종료할 때 실행된다.

- but **`break` 문으로 종료될 때는 실행되지 않는다.** (즉, `break`를 통해 중간에 종료되지 않은 경우만 실행)

  

### `pass` - 아무것도 안 함

- 문법적으로 문장이 필요하지만, 프로그램이 특별히 할 일이 없을 때 자리를 채우는 용도로 사용할 수 있다. - 스켈레톤 코드에 많이 있음.

* continue랑 헷갈리지말기