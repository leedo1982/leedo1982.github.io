---
layout  : wiki
title   : git log
summary : 로그 검색
date    : 2018-06-11 06:06:27 +0900
updated : 2018-06-11 06:16:28 +0900
tags    : git
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# Git 로그 검색
만약 where 라는 단어가 어떤 파일에 있는지 찾아보는 것이 아니라,
언제 추가되었고 히스토리 상 어느 시점에 나타나는지 찾고자 할 수 있다.

git log 명령을 이용하면 Diff 내용도 검색하여 어떤 커밋에 찾고자 하는 내용을 추가했는지 찾을 수 잇다.

ZLIB_BUF_MAX 라는 상수가 가장 처음 나타난 때를 찾는 문제라면 -S 옵셥을 이용해
해당 문자열이 추가된 커미소가 없어진 커밋만 검색할 수 있다.
```
$ git log -SZLIB_BUF_MAX --oneline
e01503b zlib: allow feeding more than 4GB in one go 
ef49a7a zlib: zlib can only process 4GB at a time
```
ef49a7a 에서 처음 나오고 e01503b에서 변경된 것을 알수 있다.

## 라인 로그 검색
진짜 미친 듯이 좋은 로그 검색도구가 있다. **라인 히스토리검색**이다.
git log를 쓸때 -L 옵션을 붙이면 어떤 함수나 한 라인의 히스토리를 볼수 있다.

예를 들어, zlib.c 파일에 있는 git_deflate_bound 함수의 모든 변경 사항 들을 보길 원한다고 생각해보다.
git log -L :git_deflate_bound:zlib.c 를 실행하면 시작과 끝을 인식해서 함수에서 일어난 모든 히스토리를 함수가 처음 만들어진 때부터 Patch를 나열하여 보여준다.

```
$ git log -L :git_deflate_bound:zlib.c
commit ef49a7a0126d64359c974b4b3b71d7ad42ee3bca 
Author: Junio C Hamano <gitster@pobox.com> 
Date: Fri Jun 10 11:52:15 2011 -0700

        zlib: zlib can only process 4GB at a time
        
diff --git a/zlib.c b/zlib.c
--- a/zlib.c
+++ b/zlib.c
@@ -85,5 +130,5 @@
-unsigned long git_deflate_bound(z_streamp strm, unsigned long size)
+unsigned long git_deflate_bound(git_zstream *strm, unsigned long size)
  {
-       return deflateBound(strm, size);
    +       return deflateBound(&strm->z, size);
  }
    commit 225a6f1068f71723a910e8565db4e252b3ca21fa
    Author: Junio C Hamano <gitster@pobox.com>
    Date:   Fri Jun 10 11:18:17 2011 -0700
    
        zlib: wrap deflateBound() too
        
    diff --git a/zlib.c b/zlib.c
    --- a/zlib.c
    +++ b/zlib.c
    @@ -81,0 +85,5 @@
    +unsigned long git_deflate_bound(z_streamp strm, unsigned long size)
    +{
    +       return deflateBound(strm, size);
    +}
+
```
git 이 함수의 처음과 끝을 인식하지 못할때는 정규표현식으로 인식하게 할수도 있다.

git log -L '/unsigned long git_deflate_bound/',/ ^}/:zlib.c

한라인의 히스토리만 검색할 수도 있고 여러 라인에 걸친 히스토리를 검색할 수도 있다.


