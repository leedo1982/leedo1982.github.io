---
layout  : wiki
title   : Git 히스토리 단장하기
summary : git history
date    : 2018-06-12 06:36:30 +0900
updated : 2019-02-09 11:24:10 +0900
tags    : git
toc     : true
public  : true
parent  : git 
latex   : false
---
* TOC
{:toc}

# History
일하다 보면 커밋 히스토리를 수정해야 할때가 있다.
결정을 나중으로 미루는 것은 git 의 장점이다.
staging Area로 커밋할 파일을 고르는 일을 커밋하는 순간으로 미룰수 있고
stash 명령으로 하던일을 미룰수 있다.

게다가 이미 커밋해서 결정한 내용을 수정할 수 있다.
순서를 변경할 수도 있고 , 커밋 메시지와 커밋한 파일도 변경할 수 있다.
여러개의 커밋을 하나로 합치거나 반대로 하나의 커밋을 여러가로 분리할 수도 있다.
아니면 커밋 전체를 삭제할 수도 있다.
**하지만, 이모든 것은 다른 사람과 코드를 공유하기 전에 해야한다.**

## 마지막 커밋 수정하기
두가지로 나눌수 있는데
하나는 커밋 메시지를 수정하는 것이고,
다른 하나는 파일 목록을 수정하는 것이다.

커밋 메세지를 수정하는 것은 간단하다.
```
$ git commit --amend
```
이때 SHA-1 값이 바뀌기 때문에 과거의 커밋을 변경할때 주의를 해야한다.
Rebase와 같이 이미 Push한 커밋은 수정하면 안된다.

## 커밋 메시지를 여러개 수정하기
현재 작업하는 브랜치에서 각 커밋을 하나하나 수정하는 것이 아니라
어느 시점부터 HEAD 까지의 커밋을 한번에 Rebase 한다.

마지막 세개의 커밋을 수정하는 것이기 때문에 ~3 으로 넘기지만,
실질적으로 가리키게 되는 것은 수정하려는 커밋의 부모인 네번째 이전 커밋이다.

```
$ git rebase -i HEAD~3
```

**다시강조하지만 이미 중앙서버에 Push한 커밋은 절대 고치치 말아야한다.**
```
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
# Rebase 710f0f8..a5f4a0d onto 710f0f8 #
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message # x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom. #
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out

```

이 커밋은 모두 log 명령과는 정반대의 순서로 나열된다.

특정 커밋에서 실행을 멈추게 하려면 스크립트를 수정해야 한다.
pick 이라는 단어를 edit로 수정하면 그 커밋에서 멈춘다.
가장 오래된 커밋 메시지를 수정하려면 아래와 같이 편집한다.

```
edit f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

저장하고 종료하면 아래와 같은 메시지를 보여주고, 명령프롬프트를 보여준다.

```
$ git rebase -i HEAD~3
Stopped at f7f3f6d... changed my name a bit 
You can amend the commit now, with
       git commit --amend
Once you’re satisfied with your changes, run
       git rebase --continue
```
정확히 뭘해야하는지 알려준다.
아래와 같이 명령을 실해하고
```
$ git commit --amend
```
커밋 메시지를 수정하고 텍스트 편집기를 종료하고 나서 아래 명령을 실행한다.

```
$ git rebase --continue
```

## 커밋 순서 바꾸기
added cat-file 커밋을 삭제하고 다른 두커밋의 순서를 변경하려면 아래와 같은 Rebase
스크립트를 

pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```
아래와 같이 수정한다.

```
pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit
```

수정한 내용을 저장하고 편집기를 종료하면 Git는 브랜치를 이 커밋의 부모로 이동시키고 310154e와 f7f3f6d 를 순서대로 적용한다.

## 커밋합치기
pick 나 edit 말고 squash 를 입력하면 git는 해당 커밋과 바로 이전 커밋을 합칠 것이고 커밋 메시지도 merge한다. 
그래서 3개의 커밋을 모두 합치려면 아래와 같이 수정한다.
```
pick f7f3f6d changed my name a bit
squash 310154e updated README formatting and added blame
squash a5f4a0d added cat-file
```
저장하고 나면 git은 3개의 커밋 메시지를 merge 할 수 있도록 에디터를 바로 실행해준다.
    
## 커밋 분리하기
기존의 커밋을 해제하고 stage를 여러개로 분리하고 나서 그것을 원하는 횟수 만큼 다시 커밋하는 것이다.

예를들어 두번째 커믓을 분리해보자.

이 커밋의 "updated RE- ADME formatting and added blame"을
"updated README formatting""과 
“added blame”으로 분리하는 것이다. 
rebase -i 스크립트에서 해당 커밋을 “edit"로 변경한다.

```
ick f7f3f6d changed my name a bit
edit 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

저장하고 나서 명령 프롬프트로 넘어간 다음에 그 커밋을 해제하고 그 내용을 다시 두개로 나눠서 커밋하면 된다.

저장하고 편집기를 종료하면 git는 제일 오래된 커밋의 부모로 이동하고
f7f3f6d 과 310154e 을 처리하고 콘솔 프롬프트를 보여준다.
그러면 수정했던 파일은 unstaged 상태가 된다.
그 다음에 파일을 Stage 한후 커밋하는 일을 원하는 만큼 반복하고 나서 
git rebase --continue 라는 명령을 실행하면 남은 Rebase 작업이 끝난다.

```
$ git reset HEAD^
$ git add README
$ git commit -m 'updated README formatting' 
$ git add lib/simplegit.rb
$ git commit -m 'added blame'
$ git rebase --continue
```

나머지 a5f4a0d 커밋도 처리되면 히스토리는 아래와 같다.
```
$ git log -4 --pretty=format:"%h %s" 
1c002dd added cat-file
9b29157 added blame
35cfb2b updated README formatting 
f3cc40e changed my name a bit
```

다시 강조하지만 rebase 를 하면 목록에 있는 SHA-1 값은 변경된다.
절대로 이미 서버에 Push한 커밋을 수정하면 안된다.


