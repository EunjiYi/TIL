200724 project1 첫 프로젝트

파이썬으로 데이터 가져오기



프로젝트란?

일정한 기간 안에 일정한 `목적을 달성`하기 위해 수행하는 업무의 묶음

하나의 프로젝트는 정해진 기간, 배정된 금액, 투입인력 등 일정한 제약조건하에서 각종 요구사항을 수행하는 방식으로 진행된다. 





open('열고싶은 파일 이름 적기', 'r', encoding = 'UTF8')

mode 종류

> r : 읽기 모드
>
> w: 쓰기 모드
>
> 아무것도 적지 않으면 기본적으로 읽기모드로 적용된다. 

encoding: 한글 때문에 파일이 정상적으로 dict으로 변환이 안되면 UTF8로 적용해서 해결한다.

 

Json : dict 같은 것

```python
import json
json_data = open('data/musics.json', encoding = 'UTF8')
dict_data = json.load(json_data)
#json 데이터(json_data)를 python에서 사용할 수 있는 dict데이터(dict_data)로 변환한다.
```

