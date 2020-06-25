---
layout  : wiki
title   : Git grep
summary : 검색하기 
date    : 2018-06-05 12:53:28 +0900
updated : 2019-02-09 11:23:27 +0900
tags    : git
toc     : true
public  : true
parent  : git
latex   : false
---
* TOC
{:toc}

# 검색 
git은 데이터베이스에 저장된 코드나 커밋에서 원하는 부분을 빠르고 쉽게 검색하는 도구가 여러가지 있다.

## git grep
기본적으로 대상을 지정하지 않으면 워킹 디렉토리의 파일을 찾는다. -n 옵션을 추가하면 차는 문자열이 위치한 라인번호도 출력한다.
```
$ git grep -n gmtime_r
compat/gmtime.c:3:#undef gmtime_r
compat/gmtime.c:8: return git_gmtime_r(timep, &result); 
compat/gmtime.c:11:struct tm *git_gmtime_r(const time_t *timep, struct tm *result) 
compat/gmtime.c:16: ret = gmtime_r(timep, result);
compat/mingw.c:606:struct tm *gmtime_r(const time_t *timep, struct tm *result) compat/mingw.h:162:struct tm *gmtime_r(const time_t *timep, struct tm *result); date.c:429: if (gmtime_r(&now, &now_tm))
date.c:492: if (gmtime_r(&time, tm)) {
git-compat-util.h:721:struct tm *git_gmtime_r(const time_t *, struct tm *); 
git-compat-util.h:723:#define gmtime_r git_gmtime_r
```
어떤 파일에서 몇개나 찾았는지 알고 싶다면 --count 옵션
매칭되는 라인이 있는 함수나 메서드를 찾고 싶다면 -p 옵션
```
$ git grep --count gmtime_r 
compat/gmtime.c:4 
compat/mingw.c:1 
compat/mingw.h:1
date.c:2
git-compat-util.h:2
```
대칭되는 라인이 있는 함수나 메서드를 찾고 싶다면 -p 옵션
```
$ git grep -p gmtime_r *.c
date.c=static int match_multi_number(unsigned long num, char c, const char *date, char *end, 
date.c: if (gmtime_r(&now, &now_tm))
date.c=static int match_digit(const char *date, struct tm *tm, int *offset, int *tm_gmt) 
date.c: if (gmtime_r(&time, tm)) {
```
--and 옵션을 이용해 여러단어가 한라인에 동시에 나타나는 줄 찾기 같은 조합을 검색할 수 있다.

git grep 가 일반적은 ack / grep 검색도구 보다 몇가지 좋은점은
우선 매우빠르다. 또한, 워킹 티렉토리만아니라 git 히스토리내의 어떠한 정보라도 찾아낼 수 있다.









































