---
layout  : wiki
title   : OddOccurrencesInArray
summary : codility
date    : 2018-05-21 08:39:52 +0900
updated : 2018-05-21 10:52:55 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
integer 로 구성된 Array A를 준다.
이 Array는 홀수로 구성되어 있으며,
각 요소는 같은 값으로 쌍을 이룬다. 
쌍을 이루지 못하는 하나의 것을 제외하고
example)
 A[0] = 9  A[1] = 3  A[2] = 9
 A[3] = 3  A[4] = 9  A[5] = 7
 A[6] = 9
 return 7

N 은 홀수 integer 로 구성되며 Array 범위는 [1..1,000,000]
각 요소의 integer 범위는 [1..1,000,000,000];
A 값 중 하나를 제외하고 모두 짝수번 발생


시간 복잡성 O(N)
공간 복잡성 O(1) : 입력 인수에 필요한 저장소 계산 안함

# point
int[] A 일때 정렬하는 방법이  
Arrays.sort(A);가 Arrays.stream(A).sort();보다 많이 빠르다.


# result 
1. Time spent : 25min (Correctness : 100%, Performance : 20%)
2. 23 min(Correctness : 100%, Performance : 50%)
3. 10 min(Correctness : 100%, Performance : 100%)



















