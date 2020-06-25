---
layout  : wiki
title   : MaxProductOfThree
summary : codility
date    : 2018-06-15 17:41:55 +0900
updated : 2018-06-17 14:48:50 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
N개의 정수로 이루어진 배열이 주어진다.
세 숫자의 곱이 제일 큰 수를 찾아라.

example)  A=[-3,1,2,-2,5,6]

## point 
sorting

제일작은 두수의 곱꽈 제일 큰 두수의 곱을 비교후 처리



## 조건
N integer range [3...100,000];
배열의 각 원소는 [-1,000...1,000]

## big-O
time O(N*log(N))
space O(1)
