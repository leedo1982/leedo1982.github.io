---
layout  : wiki
title   : rebase
summary : git rebase
date    : 2018-05-24 06:28:51 +0900
updated : 2019-02-09 11:22:02 +0900
tags    : git
toc     : true
public  : true
parent  : git
latex   : false
---
* TOC
{:toc}

# Rebase 하기
한 브랜치에서 다른 브랜치로 합치는 방법은 두가지가 있다.
하나는 Merge고 다른하나는 Rebase 다.

rebase가 실제로 일어나는 일은

일단 두 브랜치가 나뉘기 전인 공통 커밋으로 이동하고나서 그 커밋부터 지금 checkout 한 브랜치가 가리키는 커밋까지 diff를 차례로 만들어 어딘가 임시저장해 놓는다.
Rebase할 브랜치가 합칠 브래치가 가리키는 커밋을 하고 아까 저장해 놓았던 변경사항을 차례대로 적용한다.

Merge 든 Rebase든 둘다 합치는 관점에서는 서로 다를게 없다 하지만, Rebase가 좀더 깨끗한 히스토리를 만든다.
Rebase한 브랜치의 Log를 살펴보면 히스토리가 선형이다. 일을 병렬로 동시진행해도 Rebase하고 나면 모든 작업이 차례대로 수행된 것처럼 보인다.


## Rebase 활용
master에서 server 브랜치를 만들고
server 브랜치에서 client 브랜트를 만들었다.
server 브랜치를 그대로 두고 client 브랜치만 master에 합치려면
--onto 옵션을 사용하여 실행한다.
```
$ git rebase --onto master server client
```
이 명령은 client 브랜치를 checkout하고 server와 client의 공통 조상 이후의 patch를 만덜어 master에 적용한다. 
이제 master 브랜치로 돌아가서 fast-forward 시킬수 있다.
```
$ git checkout master
$ git merge client
```

server 브랜치는 일이 다끝나면 git rebase [basebranch] [topicbranch]
라는 명령으로 checkout 하지 않고 server 브랜치를 master 브랜치로 Rebase 할수 있다.
이명령은 토필(server)브랜치를 checkout 하고 베이스(master)브랜치에 Rebase 한다.


```
$ git rebase master server
```

그리고 master 브랜치로 돌아가서 fast-forward 시킬수 있다.
```
$ git checkout master
$ git merge client
```

master 브랜치에 통합됐기 때문에 client와 server 브랜치는 삭제해도 된다.

## Rebase 의 위험
**이미 공개 저장소에 push 한 커밋을 Rebase 하지마라**
이 지침만 지키면 Rebase 하는데 문제 될게 없다.
하지만 이 주의 사항을 지키지 않으면 사람들에게 욕을 먹을 것이다.


## Rebase 한것을 다시 Rebase 하기
만약 이런 상황에 빠질때 유용한 git 기능이 하나 있다.
어떤 팀원이 강제로 내가 한일을 덮어썼다고 하자. 그러면 내가 했던일이 무엇이고 덮어쓴 내용이 무엇인지 알아내야 한다.

커밋 SHA 체크섬 외에서 git는 커밋에 pathc 할 내용을 SHA 체크섬을 한번더 구한다.
이값은 'patch-id'라고 한다.
덮어쓴 커밋을 받아서 그 커밋을 기준으로 Rebase 할때 Git는원래 누가 작성한 코드인지 잘찾아낸다. 그래서 Patch가 원래대로 잘 적용된다.

git pull 명령을 실행할때 옵션을 붙여서 git pull --rebase로 rebase 할 수도 있다.

push 하기 전에 정리하려고 Rebase를 하는 것은 괜찮다. 또 절대 공개하지 않고 혼자 Rebase 하는 경우도 괜찮다.
하지만, 이미 공개하여 사람들이 사용하는 커밋을 Rebase 하면 틀림없이 문제가 생긴다.
나중에 후회말고 git pull --rebase 로 문제를 미리 방지할 수 있다는 것을 같이 작업하는 동료와 모두 함께 공유하자.

## Rebase vs Merge
rebase를 사용할지 merge를 사용할지는 각자 상황과 판단에 달렸다.

일반적인 해답을 굳이 말하자면 로컬 브런치에서는 히스토리를 정리하기 위해 rebse 할 수도 있지만 , Push 로 리모트에든 밖으로 내보낸 커밋에 대해서는 절대 rebase 하지 말아야 한다.


[출처: GitPro](https://progit2.s3.amazonaws.com/ko/2015-07-08-5c390/progit-ko.582.pdf )



