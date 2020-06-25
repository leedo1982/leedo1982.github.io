---
layout  : wiki
title   : Distinct
summary : codility
date    : 2018-06-18 11:42:46 +0900
updated : 2018-06-18 12:09:33 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
N개의 정수로 구성된 배열 A

각 숫자간의 거리를 반환하라.

ex. A [2,1,1,2,3,1]
return 3 

## 조건
N integer range [0...100,000];
배열의 각 원소는 [-1,000,000...1,000,000]

## big-O
time O(N*log(N))
space O(N)


## point
java는 단순히 set 를 활용
