---
layout  : wiki
title   : Triangle
summary : codility
date    : 2018-06-22 16:36:49 +0900
updated : 2018-06-22 16:52:13 +0900
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
triplet(P,Q,R) if 0 <= P < Q < R < N

exam.
A =[10,2,5,1,8,20]

Triplet(0,2,4)

삼각형이 존재하면 0, 아니면 1을 반환

가장 큰 숫자가 다른 두 숫자의 함보다 작으면 된다.
단 int의 범위를 넘어선 숫자의 합이 나오므로 고려해야한다.


## 조건
N integer range [0...100,000];
배열의 각 원소는 [-2,147,483,648...2,147,483,647]

## big-O
time O(N*log(N))
space O(N)

