---
layout  : wiki
title   : GenomicRangeQuery
summary : codility
date    : 2018-06-08 22:14:20 +0900
updated : 2018-06-09 17:25:19 +0900
tags    : coding
toc     : true
public  : true
parent  : coding
latex   : false
---
* TOC
{:toc}

# intro
DNA 는 A,C,G,T, 1,2,3,4 factor로 구성되어있다.

배열 P, Q가 주어지고, M 정수로 이루어져있다.

exam)
S = CAGCCTA
P =[2,5,0], Q=[4,5,6]

return [2,4,1]

## 조건 
N은 [ 1 .. 100,000 ] 범위의 정수
M은 [ 1 .. 50,000 ] 범위의 정수
배열 P, Q의 각 요소는 [ 0 .. N - 1 ] 범위 내의 정수
P [K] ≤ Q [K], 0 ≤ K <M;
문자열 S는 대문자 A, C, G, T 로만 구성


## big-O
time O(N+M)
space O(N)


## point
p와 q 배열 사이의 원소중 최소 factor을 찾아라.


