---
layout  : wiki
title   : MinAvgTwoSlice
summary : codility
date    : 2018-06-12 10:23:04 +0900
updated : 2018-06-14 11:18:20 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
N개의 정수로 구성된 배열 A가 주어진다.
(P,Q)의 쌍으로 (0 <= P < Q < N)

A = [4,2,2,5,1,5,8]
slice(1,2) avg = (2+2) / 2
slice(3,4) avg = (5+1) / 2
slice(1,4) avg = (2+2+5+1) / 4

return starting position of the slice with the minial average.

retur 1

## 조건
N integer range [2...100,000];
배열의 각 원소는 [-10,000...10,000]

## big-O
time O(N)
space O(N)

## point
javascript 로 작업할 때는 타입이 자유로워서 편하게 작성했으나.
java로 하다보니 타입에 따른 고려사항이 많아 졌다.

## result
Correctness: 100%, Performance: 100% 

