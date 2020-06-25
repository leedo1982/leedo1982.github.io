---
layout  : wiki
title   : NumberOfDiscIntersections
summary : codility
date    : 2018-06-27 13:09:17 +0900
updated : 2018-07-01 21:37:12 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro

A = [1,5,2,1,4,0]

0 =          -1 0 1
1 = -4 -3 -2 -1 0 1 2 3 4 5 6
2=              0 1 2 3 4 
3 =                 2 3 4
4 =             0 1 2 3 4 5 6 7 8
5 =                       5

0 = 3
1 = 4
2 = 2
3 = 1
4 = 1


## 조건
N integer range [0...100,000];
배열의 각 원소는 [0...2,147,483,647]

## big-O
time O(N*log(N))
space O(N)

## resutl
false
....
will find other way....later

