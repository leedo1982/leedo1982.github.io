---
layout  : wiki
title   : api 호출 한번성공? 한번실패?
summary : elb 대상그룹을 잘보자.
date    : 2019-03-22 10:01:13 +0900
updated : 2019-03-22 10:03:46 +0900
tags    : FailOver
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 증상
AWS 를 통해 개발을 하던중 api 호출이 한번은 성공하고 한번은 실패한다.

# 원인
ELB의 대상그룹에 불필요한 대상(로드 밸런싱이 필요 하지 않은 )이 추가 되어있었다.

# 그러나...
갑자기 왜 그럴까...
