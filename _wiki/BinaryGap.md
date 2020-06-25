---
layout  : wiki
title   : BinaryGap
summary : codility
date    : 2018-05-19 17:35:19 +0900
updated : 2018-06-02 10:28:23 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
일정한 숫자(N)가 주어진다.
숫자를 이진법으로 변경후 1과 1사이에 0의 최대갯수를 retrun 하여라.

예)  N = 1041 일때 결과 5

N의 범위는[1..2,147,483,647]

기대 최악의 경우의 시간 복잡도는 O (log (N))
예상 최악의 공간 복잡도는 O (1)

## point
gap 라는 점...사이라는 점을 놓치면 안됨.
테스트 케이스는 어떻게 작성할 것인가도 고민 필요.


## result 
100 %
