---
layout  : wiki
title   : MissingInteger
summary : codility
date    : 2018-05-27 17:45:29 +0900
updated : 2018-06-02 10:29:24 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
given an array A of N integers
배열에서 나오지 않은 가장 작은 양의 정수를 반환해라


example)
A = [1,3,6,4,1,2] , return 5
A = [1,2,3] return 4
A = [-1,-3] return 1


## 조건
N  integer range [1...100,000];
배열의 각원소는 integer range[-1,000,000...1,000,000]

## big-O
time O(N)
space O(N)

## point

boolean Array 를 원소의 크기 만큼 생성

배열을 돌면서 음수이면 무시하고 양수이면 
bool 배열에 true로 넣는다.

마지막으로 boolean Array에서 첫번째 false를 return 한다.

배열의크기로만 판단할수 없다.
배열 내의 가장 큰수를 꼭 확인해야함

## result
Correctness : 100%, Performance :  100%

