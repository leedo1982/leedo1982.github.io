---
layout  : wiki
title   : stash
summary : git stash
date    : 2018-05-30 21:35:09 +0900
updated : 2019-02-09 11:22:29 +0900
tags    : git
toc     : true
public  : true
parent  : git
latex   : false
---
* TOC
{:toc}

# Stashing 와 Cleaning
아직 완료하지 않은 일을 있는 상황에서 브래치를 변경해야할 경우
커밋하지 않고 나중에 다시 돌아와서 작업을 다시하고 싶을때
git stash라는 명령으로 해결할 수 있다.

stash 명령을 사용하면 워킹 디렉토리에서 수정한 파일들만 저장한다.

Stash는 Modified 이면서 Tracked 상태인 파일과
Staging Area 에 있는 파일들을 보관해두는 장소이다.

## 하던일 Stash 하기
파일 두개를 수정하고 그중 하나는 staging area에 추가한다.
그리고 git stash 하면 아래와 같은 결과가 나온다.

```
$ git status
Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
        modified:   index.html
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   lib/simplegit.rb
```
아직 작업중인 파일은 커밋할게 아니므로 stash 한다.
```
$ git stash
Saved working directory and index state \
  "WIP on master: 049d078 added the index file"
HEAD is now at 049d078 added the index file
(To restore them type "git stash apply")
```
워킹 디렉토리는 깨끗해진다.

git stash list를 사용하여 저장한 목록을 확인할 수 있다.
```
$ git stash list
stash@{0}: WIP on master: 049d078 added the index file 
stash@{1}: WIP on master: c264051 Revert "added file_size" 
stash@{2}: WIP on master: 21d80a5 added number to log
```

stash 두개는 원래 있었다.

git stash apply를 사용하여 다시 적용할 수있다.
git stash apply stash@{2} 처럼 이름을 고를 수 있으며,
이름이 없으면 가장 최근 것이 적용된다.

워킹디렉토리가 꼭 깨끗해야하는 것은아니다.
어떤 브랜치에서 stash 하고 다른 브랜치로 옮기고 거기서 stash를 복원할 수도 있다.
git stash 시 충돌이 있으면 알려준다.

git 는 stash를 적용할 때 staged 상태였던 파일을 자동으로 다시 만들어주지 않는다.
--index 옵션을 주면 staged 상태까지 적용한다.


```
$ git stash apply
# On branch master
# Changed but not updated:
# (use "git add <file>..." to update what will be committed) #
# modified: index.html
# modified: lib/simplegit.rb
```

```
$ git stash apply --index
# On branch master
# Changes to be committed:
# (use "git reset HEAD <file>..." to unstage) #
# modified: index.html
#
# Changed but not updated:
# (use "git add <file>..." to update what will be committed) #
# modified: lib/simplegit.rb 
```

apply 하여도 stash는 사라지지 않는다. 
git stash drop를 통해 제거해야한다.

## stash를 만드는 새로운 방법
주로 사용하는 옵션으로 stash save 명령과 같이 쓰이는 --keep-index 이다.
이 옵션은 이미 Staging Area에 들어있는 파일을 Stash 하지 않는다.
많은 파일을 변경했지만 몇몇 파일만 커밋하고 나머지 파일은 나주에 처리하고 싶을때 유용하다.

```
$ git status -s 
M index.html
M lib/simplegit.rb

$ git stash --keep-index
Saved working directory and index state WIP on master: 1b65b17 added the index fil HEAD is now at 1b65b17 added the index file

$ git status -s 
M index.html

```
추척하는 파일과 추적하지 않는 파일을 같이 stash하는 일도 빈번한다.
기본적으로 git stash는 추적중인 파일만 저장한다.

끝으로 --patch 옵션을 붙이면 Git는 수정된 모든 사항을 저장하지 않는다.
대신 대화형 프롬프트가 뜨며 변경된 데이터 중 저장할 것과 저장하지 않을 것을 지정할 수 있다.

```
$ git stash --patch
diff --git a/lib/simplegit.rb b/lib/simplegit.rb index 66d332e..8bb5674 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -16,6 +16,10 @@ class SimpleGit
             return `#{git_cmd} 2>&1`.chomp
           end
    end +
+
+
+ end
def show(treeish = 'master')
  command("git show #{treeish}")
    end
    test
Stash this hunk [y,n,q,a,d,/,e,?]? y
Saved working directory and index state WIP on master: 1b65b17 added the index file


```


## Stash를 적용한 브런치 만들기
git stash branch 명령을 실행하면 Stash할 당시의 커밋을 Checkout 한후 새로운 브런치를 만들고 여기에 적용한다. 이 모든 것이 성공하면 stash 를 삭제한다.

```
$ git stash branch testchanges
Switched to a new branch "testchanges"
# On branch testchanges
# Changes to be committed:
# (use "git reset HEAD <file>..." to unstage)
#
# modified: index.html
#
# Changed but not updated:
# (use "git add <file>..." to update what will be committed)
#
# modified: lib/simplegit.rb
#
Dropped refs/stash@{0} (f0dfc4d5dc332d1cee34a634182e168c4efc3359)

```

## 워킹 디렉토리 청소하기
stash 하지 않고 단순히 그 파일들을 지워버리고 싶을때, git clean
이 명령어는 신중해야한다. 워킹디렉토리 안의 추적하고 있지 않은 모든 파일이 지워지기 때문이다.
명령을 실행하고 나서 후회해도 소용없다.
git stash -all 명령을 이용하면 지우는 것은 동일하다지만 , 모든 파일을 stash 하므로 안전하다.


