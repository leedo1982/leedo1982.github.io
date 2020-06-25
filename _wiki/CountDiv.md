---
layout  : wiki
title   : Count Div
summary : codility
date    : 2018-06-03 18:39:44 +0900
updated : 2018-06-04 10:10:11 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
세개의 integer A,B,K rk wndjwlsek.
range[A,B] 사이에 K로 나누어 떨어지는 수를 반환힌다.

A = 6, B = 11, K =2
return 3 //g[6,8,10]


## 조건
A,B integer range [0...2,000,000,000];
K integer range [1...2,000,000,000];
A <= B


## big-O
time O(1)
space O(1)


## point 
0의 특수성으로 인한 고려가 필요하다.
분자가 무조건 0이면 나머지가 0이므로 이를 고려해서 반영해야한다.

## result
Correctness: 100%, Performance: 100%

