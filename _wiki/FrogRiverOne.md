---
layout  : wiki
title   : FrogRiverOne
summary : codility
date    : 2018-05-27 17:05:25 +0900
updated : 2018-06-02 10:29:15 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
given an array A consisting of N integers
최소값을 찾아라 반대편으로 가기 위한
모든 position을 건너야 한다. 1부터 X까지

example) A[1,3,1,4,2,3,5,4], X = 5, return 6

## 조건
N 과 X 는 integer range [1...100,000];
배열의 각원소는 integer range[1...X]

## big-O
time O(N)
space O(X)


## point

1~X 까지의 합을 구하고

1~X까지의 boolean의 배열을 만들고

bool 배열에 해당 숫자가 false면 sum에서 빼고 true면 넘어가고

sum이 0이면 해당 index , sum이 남으면 -1 리턴


## result
Correctness : 100%, Performance : 100%
