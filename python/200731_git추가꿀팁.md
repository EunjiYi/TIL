200731 git추가꿀팁



git status찍으면, 메세지에 `staged`라는게 나오는데

 staged = `스테이징에어리어에 올리는 작업 = 즉 git add해라.`



**git add . 해서 한 번에 다 올렸을 때 취소하는 법**

`git restore --staged b.txt`

(예전버전은 git reset이라던데 나는 restore로 됐다.)



**만약 커밋을 했는데 오타를 냈다!**

(그전에 기본적으로 항상 커밋은 굉장히 신중하게 하는 게 좋다.)

` git commit --amend`

​	*amend는 가장 마지막 커밋만 가능*

add b.txtttttt로 오타낸 걸 => add b.txt로 바꾸기



**마지막커밋(b.txt만 들어있는 커밋)에 c,txt를 추가하자.**

git commit --amend로 

add b.txt -> add b,c.txt 로 수정하기



**git log 한 줄로 간편하게 보기**

한줄로 볼 수 있는 옵션 

`git log --oneline`

commit 앞에 7글자만 나옴. ex) 0cd4db3



**수많은 파일중에 뭐가 바뀌었는지 확실히 비교**

`git diff`

> 만약 a.txt가 git add로 스테이징 에어리어에 있다고 생각하자. b.txt는 아직 워킹디렉토리에 있고. 
>
> 이 상태일 때 diff는 워킹디렉토리 안에 있는 것만 서로 비교
>
> 즉, git diff는 `add하기 전에 확인하는 용도`이다.





**git add 한 거 취소하기**

`git rm --cached 취소하려는파일명`

`git rm --cached __pycache__/kobis.cpython-37.pyc`



**가장 최근 상태로 돌아가면서 커밋 자체를 취소해줌**

`git reset HEAD^` 

*이렇게 해주시면 커밋이 취소 되거든요*



+ 추가 질문

  > 커밋한것 중에 특정파일 하나만 커밋 취소하려면 어떻게 해야되나요?
  >
  > - 새로운 메시지
  >
  > ​	전체 커밋 취소하고 다시 해당 파일을 삭제해야 해요. 파일 하나만 취소는 할 수 없어요.





#### 총정리

- 버전관리 

add

commit

push



- 상태확인

status

log

diff



- 되돌리기 (알고만 있고 되도록 쓰지말자.)

restore --amend 



끝