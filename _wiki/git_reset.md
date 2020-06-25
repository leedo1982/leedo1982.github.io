---
layout  : wiki
title   : git reset
summary : 명확히 알고가기
date    : 2018-06-14 06:42:07 +0900
updated : 2019-02-09 11:24:25 +0900
tags    : git reset
toc     : true
public  : true
parent  : git
latex   : false
---
* TOC
{:toc}

# reset
Git 을 처음 사용하는 사람에게 가장 헷갈리는 부분이다.
제대로 이해하고 쉽게 사용하자

## 세걔의 트리
git 을 서로다른 세 트리를 관리하는 컨텐츠 관리자로 생각하면 reset와 checkout을 좀더 쉽게 이해할 수 있다(트리 == 파일의 묶음)
세 트리 중 index는 트리가 아니지만, 이해를 쉽게하려 일단 트리라고 한다.


| 일반적인 기능                     | Tree              |
| ----------                        | --------------    |
| Role                              | HEAD              |
| Last Commit snapshot, next parent | Index             |
| Proposed next commit snapshot     | Working Directory |
| SandBox                           | 트리              |


### HEAD
현재 브랜치를 가리키는 포인터이면, 브랜치는 브랜치에 담긴 커밋 중 가장 마지막 브런치를 가리킨다.
지금의 HEAD가 가리키는 커밋은 바로 다음 커밋의 부모가 된다.
단순하게 생각하면 HEAD 는 마지막 커밋의 스냅샷이다.

HEAD 스냅샷의 디렉토리 리스팅과 각 파일의 SHA-1 체크섬을 보여주는 예제
```
$ git cat-file -p HEAD
tree cfda3bf379e4f8dba8717dee55aab78aef7f4daf 
author Scott Chacon 1301511835 -0700 
committer Scott Chacon 1301511835 -0700

initial commit

$ git ls-tree -r HEAD
100644 blob a906cb2a4a904a152... README
100644 blob 8f94139338f9404f2... Rakefile
040000 tree 99f1a6d12cb4b6f19... lib
```

cat-file 와 ls-tree 명령은 일상적으로는 잘 사용하지 않는 저수준 명령이다.
이러한 저수준 명령을 plumbing 명령이라고 한다.
Git이 실제로 무순일을 하는지 볼때 유용하다.

### INDEX
바로 다음에 커밋할 것들이다.
이러한 개념을 Stagin Area 라고 배운바 있다.
"Stageing Area"는 사용자가 git commit 명령을 실행했을때 git 이 처리할 것들이 있는곳이다.
index는 워킹 디렉토리에서 마지막으로 checkout 한 브랜치의 파일 목록과 파일 내용으로 최워진다.
```
$ git ls-files -s
100644 a906cb2a4a904a152e80877d4088654daad0c859 0 README
100644 8f94139338f9404f26296befa88755fc2598c289 0 Rakefile
100644 47c6340d6459e05787f644c2447d2595f5d3a54b 0 lib/simplegit.rb
```

ls-files 명령은 훤신 더 장막 뒤에 가러져 있는 명령으로 
이를 실행하면 현재 index가 어떤 상태인지를 확인할 수 있다.

### 워킹 디렉토리
위의 두 트리는 파일과 그 내용을 효율적인 형태로 .git 디렉토리에 저장한다.
하지만 사람이 알아보기 어렵다. 워킹 디렉토리는 사람 눈에 보이기 때문에 편집에 수월하다.
위킹 디렉토리를 샌드박스로 생각하자
커밋하기 전에는 index(Staging Area)에 올려 놓고 올마든지 변경 할 수 있다.


### workflow
git 의 주목적은 프로젝트의 스냅샷을 지속적으로 저장하는 것이다.
이트리 세개를 사용해 더나은 상태로 관리한다.

하나의 파일이 있는 곳에서 git init를 하면
git 저장소가 생기고 HEAD는 아직 없는 브런치를 가리킨다.(working directory)

이제 git add 를 하면 working directory 내용을 index로 복사한다.

git commit 를 하면 index의 내용을 스냅샷으로 영구히 저장하고, 그 스냅샷을 가리키는 커밋 객체를 만든다. 그리고 master가 그 커밋 객체를 가리키도록 한다.

이때 git status는 아무런 변경사항이 없다고 나온다. 세 트리 모두 같기 때문에.

브랜치를 바꾸거나 Clone 명령도 내부에서는 비슷한 절차를 밟는다.
브랜치를 checkout 하면 Head가 새로운 브랜치를 가리키도록 바뀌고, 새로운 커밋의 스냅샷을 index에 놓는다. 그리고 index의 내용을 워킹 디렉토리로 복사한다.

### Rest의 역할
reset 명령은 세 트리를 간단하고 예측 가능한 방법으로 조작한다. 
트리를 조작하는 동작은 세단계 이하로 이루어진다.

1단계 : HEAD 이동
reset 명령이 하는 첫번째 일은 HEAD 브랜치를 이동시킨다.
checkout 처럼 HEAD가 가리치는 브랜치를 바꾸지는 않는다.

HEAD는 계속 현재 브랜치를 가리키고, 현재 브랜치가 가리키는 커밋을 바꾼다.
git reset --soft 옵션을 사용하면 딱 여기까지 진행하고 동작을 멈춘다.

2단계 : INDEX 업데이트(--MIXED)
reset 명령은 여기서 한발짝 더 나아가 Index를 현재 HEAD가 가리키는 스냅샷으로 업데이트 할 수 있다.
--mixed 옵션을 주고 실행하면 여기까지 하고 멈춘다.

3단계: 워킹 디렉토리 업데이트(--HEAD)
reset 명령은 세번째로 워킹 디렉토리까지 업데이트 한다. 
--hard 옵션을 사용하면 이단계까지 수행한다.
--hard 옵셩은 매우 중요하다. rest 명령을 위험하게 만드는 유일한 옵션이다.
git에서는 데이터를 실제로 삭제하는 방법이 별로 없다.
이 삭제하는 방법은 그 중하나다.
git이 커밋을 보관하고 있기 때무에 reflog를 이용해서 다시 복원할 수 있다.
만약 커밋한 적이 없다면 git이 덥어쓴 데이터는 복원할 수 없다.

### 경로를 주고 Reset 하기
reset 명령을 실행할 때 경로를 지정하면 1단계를 건너뛰고 정해진
경로의 파일에만 나머지 reset 단계를 적용한다.

예를 들어 git reset file.txt 명령을 실행한다 가정하자.
이는 git reset --mixed HEAD file.txt 를 짧게 쓴것이다.
