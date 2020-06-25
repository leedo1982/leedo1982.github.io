---
layout  : wiki
title   : PassingCars
summary : codility
date    : 2018-06-04 10:25:45 +0900
updated : 2018-06-06 10:16:16 +0900
tags    : coding
toc     : true
public  : true
parent  : Coing
latex   : false
---
* TOC
{:toc}

# intro
N integer로 구성된 배열이 주어진다.
배열에는 오직 0/1이 있다.

0 => east
1 => west

몇대의 차를 지나갔는지가 count 하는 것이 목적이다.
차는 (P,Q)로주어지며 0<= P < Q < N

example

A[0,1,0,1,1]
passing cars (0, 1), (0, 3), (0, 4), (2, 3), (2, 4)
return 5

return number of passing cars

만약 지나간 차가 1,000,000,000 이면 -1 반환


처음에 문제가 이해 안갔다.
독해능력이 더 필요할 듯...

## 조건
N integer range [0...100,000];
배열의 각 원소는 0,1


## big-O
time O(N)
space O(1)

## result
Correctness: 100%, Performance: 100% 
