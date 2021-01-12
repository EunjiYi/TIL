# Git 사용법



## Git기본

> `working directory -> staging area -> local repository -> remote repository`

* `git add` : working directory -> staging area
* `git commit` : staging area -> local repository
* `git push` : local repository -> remote repository
* `git clone [url]` : 원격 저장소에서 로컬로 파일 복사해오기
* `rm -rf .git` : 쌓았던 커밋 모두 지우기
* `git log --oneline` : git 로그를 한줄로 확인



## 메인

* Git에서 브랜치 쓰기
  * git branch [브랜치명] : 해당 이름의 브랜치를 딴다.
    * ex) `git branch feature/nav`
  * git branch : 생성된 브랜치 목록 확인
  * git branch -d [브랜치명] : 해당 브랜치 삭제
  * git checkout [브랜치명] : 해당 브랜치로 이동, 최근에는 checkout의 의미가 명확하지 않아 switch로 개선
  * git switch [브랜치명] : 해당 브랜치로 이동
* Git에서 브랜치 합치기(Merge)
  * git merge [브랜치명] : 현재 브랜치에 해당 브랜치를 병합하기
* 리베이스(rebase)의 기본
  * git rebase [브랜치명] : 현재 브랜치의 커밋을 해당 브랜치의 커밋 아래로 옮기기(커밋을 한줄로 만들 수 있다.)
* HEAD 분리하기
  * HEAD는 현재 체크아웃된 커밋을 가리킨다.(현재 작업 중인 커밋)
  * 일반적으로 HEAD는 브랜치의 이름을 가리키고 있다.
  * 커밋의 해시값을 이용해 HEAD를 분리할 수 있다.
    * ex) `git switch [해시값]`
* 상대 참조(^)(캐롯문자)
  * 한번에 한 커밋 위로 움직인다.
    * `git switch HEAD^`
* 상대 참조(~[num])(틸드문자)
  * 한번에 여러 커밋 위로 올라간다.
    * `git switch HEAD~3` : 3번 위로 올라간다.
* 브랜치의 위치를 강제로 이동하기
  * `git branch -f master HEAD~3` : master브랜치의 위치를 강제로 위로 3번 올리기
* Git에서 작업 되돌리기
  * `git reset`(local 브랜치에서 사용)
    * ex) `git reset HEAD~1`
  * `git revert`(remote 브랜치에서 사용)
    * ex) `git revert HEAD`



## 원격

* 원격브랜치
  * 로컬 저장소와 원격 저장소가 연결되면 로컬에 생성되는 브랜치
  * 원격 저장소의 상태를 반영하는 브랜치
  * 분리된 HEAD모드이다.
  * 원격 저장소가 갱신될때만 갱신된다.
* fetch
  * `git fetch` : 원격 저장소에서 데이터를 가져오는 방법
    * 원격 저장소에는 있지만 로컬에는 없는 커밋들을 다운로드 한다.
    * 원격 브랜치가 가리키는 곳을 업데이트 한다.
    * 로컬의 상태는 바꾸지 않는다.
* pull
  * fetch + merge
  * `git pull --rebase` : merge가 아닌 rebase로 브랜치가 합쳐진다.
* push
  * 로컬 --> 원격으로 저장
* 엇갈린 히스토리
  * push를 하기전에 `git fetch`로 원격 저장소의 변경정보를 가져오고 리베이스 한후 push한다.
  * `git fetch` -> `git rebase origin/master` -> `git push`
  * fetch + rebase 명령어인 `git pull --rebase`로도 가능
    * `git pull -rebase` -> `git push`
  * merge로도 가능
  * `git fetch` -> `git merge origin/master` -> `git push`
    * = `git pull` -> `git push`
* 잠겨버린 Master(Remote Rejected)
  * Pull Request를 해야되는데 그냥 push를 한 경우 발생
  * 해결책
    * `feature` 등 다른 이름으로 브랜치를 만들고 원격 저장소에 push한다.
    * 원격 저장소와 로컬이 동기화 될 수 있도록 로컬의 master 브랜치를 reset한다.

