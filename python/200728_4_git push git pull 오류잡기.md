200728 git push git pull 오류 잡기



git pull origin master할 때 

fetal: refusing to merge unrelated histories

뜬다면, 



git pull origin master --allow-unrelated-histories치고

esc

:q

하고 엔터치고 다시 git pull origin master 하면 집 컴퓨터랑 gitlab서버랑 연동완료



```ptyhon
git push하는데 안되서
git pull 먼저 하려는데도 오류 날때
$ git pull origin master --allow-unrelated-histories
이렇게 하고 다시 push하기

EUNJI YI@LAPTOP-ON7NAJRQ MINGW64 ~/Desktop/commit (master)
$ git push origin master
To https://github.com/EunjiYi/project_practice.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/EunjiYi/project_practice.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
EUNJI YI@LAPTOP-ON7NAJRQ MINGW64 ~/Desktop/commit (master)

$ git pull origin master --allow-unrelated-histories
From https://github.com/EunjiYi/project_practice
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 README.md | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 README.md
EUNJI YI@LAPTOP-ON7NAJRQ MINGW64 ~/Desktop/commit (master)

$ git push origin master
Enumerating objects: 17, done.
Counting objects: 100% (17/17), done.
Delta compression using up to 4 threads
Compressing objects: 100% (16/16), done.
Writing objects: 100% (16/16), 5.07 KiB | 185.00 KiB/s, done.
Total 16 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (3/3), done.
To https://github.com/EunjiYi/project_practice.git
   11a8ec0..f482ad4  master -> master
```



