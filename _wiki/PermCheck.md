---
layout  : wiki
title   : permCheck
summary : codility
date    : 2018-05-31 10:38:05 +0900
updated : 2018-05-31 12:22:48 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
array A of N integers is given

순열은 1에서 N까지의 각 요소를 한 번만 포함하는 시퀀스입니다.

qㅐ열이 주어질때 순열이면은 1, 아니면 0을 반환한다.

A[4,1,3,2]  return 1
A[4,1,3]    return 0

## 조건
N  integer range [1...100,000];
배열의 각원소는 integer range[1...1,000,000,000]

## big-O
time O(N)
space O(N)

## point 

수학적인 합과 배열의 합이 동일하면 1, 아니면 0

중간에 숫자가 임의적으로 들어간다는 점.

## result
Correctness : 100%, Performance :  100%
