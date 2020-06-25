---
layout  : wiki
title   : PermMissingElem
summary : codility
date    : 2018-05-23 13:28:32 +0900
updated : 2018-05-23 16:16:35 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro 
  서로 다른 integer로 구성된 A 배열이 주어진다.
  배열에 포함된 integer의 범위는 [1...(N+1)]이며
이것은 정확히 하나의 요소가 빠졌다는 것을 의미한다.

우리의 목표는 빠진 한 요소를 찾는 것이다.

example
A[0] = 2, A[1] =3, A[2] = 1, A[3]= 5 
return 4

조건
N 은 integer 이며 범위는 [0...100,000]
A 배열의 요소는 모두 distinct 하다.
각 요소의 범위는 [0...(N+1)]


time complexity  : O(N)
space complexity : O(1)

# point
1 ~ n까지의 합을 이용하여 푼다.
요소들의 총합을 이용하면 쉬우나.

진정한 문제는 primary type의 볌위를 잘 생각하고 풀어줘여한다.
주어진 요소의 범위가 클경우 int type를 넘어서기 때문에 올바른 답이 나오지 않는다.

# result
Correctness : 100%
Performance : 100%
