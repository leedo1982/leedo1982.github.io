---
layout  : wiki
title   : Fish
summary : Codility
date    : 2018-07-09 16:07:01 +0900
updated : 2018-07-09 16:35:07 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
두개의 정수 배열을 준다.

물고기의 수는 0부터 N-1 이다.

만약 P,Q가 P<Q 이면 P는 초기에 Q의 상류에 있다. 각 물고기는 유니크한 위치가 있다.

배열 A는 물고기의 크기, 모든 원소는 유니크
배열 B는 방행을 포함한다. 0(upstream) , 1(downstream)

크기가 큰 물고기가 잡아먹는다.
남은 물고기의 수를 반환하라

## assume
N은 [1..100,000] 범위의 정수입니다.
배열 A의 각 요소는 [0..1,000,000,000] 범위의 정수입니다.
배열 B의 각 요소는 다음 값 중 하나를 가질 수있는 정수입니다 : 0, 1;
A의 요소는 모두 구별됩니다.

## Bic-O
시간 : O (N), 공간 O(N)

## result



