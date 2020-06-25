---
layout  : wiki
title   : git merge
summary : 고급 Merge
date    : 2018-06-25 17:39:08 +0900
updated : 2019-02-09 11:27:17 +0900
tags    : git
toc     : true
public  : true
parent  : git
latex   : false
---
* TOC
{:toc}

# intro
git의 철학은 Merge가 잘 될지 아닐지 판단하는 것을 잘하자 이다.
충돌이 나도 자동으로 해결하려고 노력하지 않는다.
오랫동안 따로 유지한 두 브랜치를 Merge 하려면 몇가지 해야할 일이 있다.

## Merge 충돌
Merge 할 때에는 충돌이 날 수 있어서, 하기전에 워킹 디렉토리를 깔끔히 정리하는 것이 좋다.
임시 브랜치에 커밋하거나 stash 해둔다.

예제, hello world를 출력하는 Ruby 파일이 있다.
```
#! /usr/bin/env ruby

def hello
puts 'hello world'
end

hello()
```

whitespace 브랜치를 생성하고 위의 파일에서 모든 Unix 형식 개행을 DOS 형식 개행으로 바꾸어 커밋힌다.
파일의 모든 라인이 바뀌었지만, 공백만 바뀌었다.
그후 "hello world" 문자열을 "hello mundo" 로바꾸고 커밋한다.
```
$ git checkout -b whitespace 
Switched to a new branch 'whitespace'

$ unix2dos hello.rb
unix2dos: converting file hello.rb to DOS format ... 
$ git commit -am 'converted hello.rb to DOS' 
[whitespace 3270f76] converted hello.rb to DOS
 1 file changed, 7 insertions(+), 7 deletions(-)
 
$ vim hello.rb
$ git diff -b
diff --git a/hello.rb b/hello.rb 
index ac51efd..e85207e 100755 --- a/hello.rb
+++ b/hello.rb
@@ -1,7 +1,7 @@
 #! /usr/bin/env ruby
 
 def hello
-  puts 'hello world'
+  puts 'hello mundo'^M
end

hello()

$ git commit -am 'hello mundo change' 
[whitespace 6d338d2] hello mundo change
 1 file changed, 1 insertion(+), 1 deletion(-)
```

master 브랜치로 다시 이동한 다음 함수에대한 설명을 추가한다.
```
$ git checkout master 
Switched to branch 'master'

$ vim hello.rb
$ git diff
diff --git a/hello.rb b/hello.rb 
index ac51efd..36c06c8 100755 --- a/hello.rb
+++ b/hello.rb
@@ -1,5 +1,6 @@
 #! /usr/bin/env ruby
 
+# prints out a greeting
 def hello
   puts 'hello world'
 end
 
$ git commit -am 'document the function' 
[master bec6336] document the function
 1 file changed, 1 insertion(+)

```

이때 whitespace 브랜치를 merge 하면 공백변경 탓에 충돌이 난다.
```
$ git merge whitespace
Auto-merging hello.rb
CONFLICT (content): Merge conflict in hello.rb
Automatic merge failed; fix conflicts and then commit the result.
```

##Merge 취소하기
예상하고 있던일도 아니고 지금 당장 처리할 일도 아니라면 
git merge --abort 명령으로간단히 Merge 하기 전으로 되돌린다.
```
$ git status -sb 
## master
UU hello.rb

$ git merge --abort
$ git status -sb 
## master
```

완전히 뒤로 되돌리지 못하는 유일한 경우는 Merge 전에 워킹 디렉토리에서 Stash하지 않았거나 커밋하지 않은 파일이 있었을 때 뿐이다.

어떤 이유로든 merge를 처음부터 다시하고 싶다면 
git reset --hard HEAD 명령으로 되돌릴 수 있다.

## 공백무시하기
기본 Merge 전략은 공백의 변화는 무시하도록 하는 옵션을 주는 것이다.

```
$ git merge -Xignore-space-change whitespace 
Auto-merging hello.rb
Merge made by the 'recursive' strategy.
 hello.rb | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

```
위 예제는 모든 공백변경 사항을 무시하면 실제 파일은 충돌나지 않고 모든 Merge가 잘 실행된다.
팀원 중 누군가 스페이스를 탭으로 바꾸거나 탭을 스페이스로 바꾸는 짓을 했을때 이 옵션이 그대를 구원해 준다.


## 수동으로 MERGE 하기
git 이 자동으로 해결해 주지 못하는 상황에 부닥치면 직접 손으로 해결해야 한다.

파일을 dos2unix로 변환하고 Merge하면 된다.
이걸 git에서 어떻게 하는지 살펴보자.
먼저 Merge 충돌 상태에 있다고 치자.
현 시점의 파일과 Merge 할 파일, 공통 조상의 파일이 필요하다.
이파일들로 어쨌든 잘 Merge 되도록 수정하고 다시 Merge 를 시도해야한다.

우선 세가지 버전의 파일을 얻는 건 쉽다.
git은 세 버전의 모든 파일에 "stages" 숫자를 붙여서 index에 다 가지고 있다.
Stage 1는 공통 조상 파일,
Stage 2는 현재 개발자의 버전에 해당하는 파일조상 ,
Stage 3는 MERGE_HEAD가 가리키는 파일이다. 

git show 명령으로 각 버전의 파일을 꺼낼 수 있다.
```
$ git show :1:hello.rb > hello.common.rb 
$ git show :2:hello.rb > hello.ours.rb 
$ git show :3:hello.rb > hello.theirs.rb
```

공백문제를 수동으로 고친다음에 다시 merge 한다.
merge 할때는 git merge-file 명령을 이용한다.
```
$ dos2unix hello.theirs.rb
dos2unix: converting file hello.theirs.rb to Unix format ...

$ git merge-file -p \
hello.ours.rb hello.common.rb hello.theirs.rb > hello.rb

$ git diff -b
diff --cc hello.rb
index 36c06c8,e85207e..0000000 
--- a/hello.rb
+++ b/hello.rb
@@@ -1,8 -1,7 +1,8 @@@
  #! /usr/bin/env ruby
 +# prints out a greeting
  def hello
-   puts 'hello world'
+   puts 'hello mundo'
end 

hello()

```

merge 커밋을 완료하기 전에 양쪽 부모에 대해서 무엇이 바뀌었는지 확인하려면 git diff를 사용한다.
merge 후의 결과를 Merge 하기 전의 브랜치와 비교하려면, 다시 말해 무엇이 합쳐졌는지 알려면
git diff --ours 명령을 실행한다.

```
$ git diff --ours
* Unmerged path hello.rb
diff --git a/hello.rb b/hello.rb 
index 36c06c8..44d0a25 100755 --- a/hello.rb
+++ b/hello.rb
@@ -2,7 +2,7 @@

 # prints out a greeting
 def hello
-  puts 'hello world'
+  puts 'hello mundo'
end 

hello()

```
merge 할 파일을 가져온 쪽과 비굫서 무엇이 바뀌었는지 보려면
gitdiff --theirs를 실행한다. 아래 예제는 공백을 빼고 비교하기 위해 -d 옵션을 같이 써주었다.

```
$ git diff --theirs -b
* Unmerged path hello.rb
diff --git a/hello.rb b/hello.rb
index e85207e..44d0a25 100755
--- a/hello.rb
+++ b/hello.rb
@@ -1,5 +1,6 @@
 #! /usr/bin/env ruby
 
+# prints out a greeting
 def hello
   puts 'hello mundo'
 end

```
마지막으로 git diff --base 를 사용해서 양쪽 모두와 비교하여 바뀐점을 알아본다.

```
$ git diff --base -b
* Unmerged path hello.rb
diff --git a/hello.rb b/hello.rb 
index ac51efd..44d0a25 100755 --- a/hello.rb
+++ b/hello.rb
@@ -1,7 +1,8 @@
 #! /usr/bin/env ruby
 
+# prints out a greeting
 def hello
-  puts 'hello world'
+  puts 'hello mundo'
end 

hello()
```

수동 merge 를 위해서 만들었던 각종 파일은 git clean을 실행해서 지워준다.

```
$ git clean -f
Removing hello.common.rb 
Removing hello.ours.rb 
Removing hello.theirs.rb
```
## 충돌 파일 CHECKOUT
예제를 조금 바꾸어
긴 호흡의 브랜치 두개가 있다. 각 브랜치에는 몇개의 커밋이 있는데 양쪽은 merge 할때
반드시 충돌이 날 만한 내용이 들어있다.
```
$ git log --graph --oneline --decorate --all 
* f1270f7 (HEAD, master) update README
* 9af9d3b add a README
* 694971d update phrase to hola world
| * e3eb223 (mundo) add more tests
| * 7cff591 add testing script
| * c3ffff1 changed text to hello mundo
|/
* b7dcc89 initial hello world code

```
master 에만 있는세개의 커밋과 mundo 브랜치에만 존재하는 또 다른 세개의 커밋이 있다.
master 브랜치에서 mundo 브랜치를 Merge 하면 충돌이 난다.
```
$ git merge mundo
Auto-merging hello.rb
CONFLICT (content): Merge conflict in hello.rb
Automatic merge failed; fix conflicts and then commit the result.
```

해당 파일을 열면 아래와 같다.
```
#! /usr/bin/env ruby
def hello 
<<<<<<< HEAD
     puts 'hola world'
=======
puts 'hello mundo' 
>>>>>>> mundo
end

hello()
```
양쪽 브랜치에서 추가된 부분이 이 파일에 다 적용됐다.


git checkout 명령에 --conflict 옵션을 붙여 사용하는게 좋은 방법이 될 수 있다.
이 명령은 파일을 다시 Checkout 받아서 충돌 표시된 부분을 교체한다.
충돌 난 부분은 원래의 코드로 되돌리고 다시 고쳐보려고 할 때 알맞은 도구다.

--conflict 옵션에는 diff3나 merge를 넘길 수 있고 merge 가 기본값이다.
diff3 명령에 --conflict 옵션을 사용하면 git는 약간 다른 충돌모양을 제공한다.


```
$ git checkout --conflict=diff3 hello.rb

#! /usr/bin/env ruby
def hello 
<<<<<<< ours
     puts 'hola world'
||||||| base
     puts 'hello world'
=======
puts 'hello mundo' 
>>>>>>> theirs
end

hello()

```
git checkeout 명령도 --ours 와 --theirs 옵션을 지원한다.

## MERGE 로그
git log 명령은 충돌을 해결할 때도 도움이 된다.

맥락에 따라 필요한 결과만 추려 볼 수도 있다.
git log 명령에 --merge 옵션을 추가하면 충돌이 발생한 파일이 속한 커밋만 보여준다.
```
$ git log --oneline --left-right --merge 
< 694971d update phrase to hola world
> c3ffff1 changed text to hello mundo
```

--merge 대신 -p 를 사용하면 충돌 난 파일의 변경사항만 볼수 있다.
이건 왜 충돌이 났는지 또 이를 해결하기 위해 어떻게 해야하는지 이해햐는데 진짜로 중요하다.


## Merge 되돌리기
### REFS 수정
실수로 생긴 Merge 커밋이 로컬 저장소에만 있을 때에는 브런치를 원하는 커밋을 가리키도록 옮기는 것이 쉽고 빠르다.

git reset --hard HEAD~ 명령으로 되돌리면 된다.

이 방법의 단점은 히스토리를 다시 씉다는 것이다.
### 커밋 되돌리기
브랜치를 옮기는 것을 할 수 없는 경우는 모든 변경사항을 취소하는 새로운 커밋을 만들 수도 있다.

```
$ git revert -m 1 HEAD

```

-m 1 옵션은 부모가 보호되어야하는 mainline 라는 것을 나타낸다.

