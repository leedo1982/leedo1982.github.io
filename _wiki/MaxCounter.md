---
layout  : wiki
title   : MaxCounter
summary : codility
date    : 2018-06-02 10:30:24 +0900
updated : 2018-06-02 12:16:00 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
처음엔 0으로 설정된 카운터가 주어진다.
 두가지 기능이 있다.
    increase(X) - 1씩 증가.
    max counter - 모든 counters 의 최대값을 설정
    
if A[K] = X, 1<= X <=N , increase(X)
if A[K] = N+1, K 는 max counter


example. integer N = 5, array A[3,4,4,6,1,4,4]
    (0, 0, 1, 0, 0)
    (0, 0, 1, 1, 0)
    (0, 0, 1, 2, 0)
    (2, 2, 2, 2, 2)
    (3, 2, 2, 2, 2)
    (3, 2, 2, 3, 2)
    (3, 2, 2, 4, 2)

return [3,2,2,4,2]

## 조건
N, M integer range [1...100,000];
배열의 각원소는 integer range[1...N+1]

## big-O
time O(N+M)
space O(N)

## point 
A의 순서는 중요하다. 
max 값을 보관하고 있어야 한다.

A[K] 가 N+1이 되는 때 배열을 리셋한다.

foreach fori나 performance랑 관련 없다.
변수대입도 그런듯.


performance 가 80%에서 멈춰서 시간이 걸렸다.

원인은 new를 통한 객체생성이 생각보다 시간을 많이 잡아먹는 것 같다.

for 문안에서 new를 통한 객체 생성을 자제하고,
System.arraycopy 를 통해 사용하는 것이 시간을 아껴준다.

## result
Correctness: 100%, Performance: 100%

