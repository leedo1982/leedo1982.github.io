---
layout  : wiki
title   : TapeEquilibrium
summary : codility
date    : 2018-05-26 07:17:19 +0900
updated : 2018-06-02 10:29:02 +0900
tags    : coding
toc     : true
public  : true
parent  : Coding
latex   : false
---
* TOC
{:toc}

# intro
N 정수들로 구성된 배열이 주어진다.

어떤 정수 P(0<P<N) 로 splits 하여 두개의 부분으로 나누고
두 부분의 합의 차가 가장 작은 결과(절대값)를 반환하라

example) A[3,1,2,4,3]
P =1, |3 - 10| = 7
P =2, |4 -  9| = 5
**P =3, |6 -  7| = 1**
P =4, |10 - 3| = 7

resutl = 1

가정,
N은 정수이며 range [2...100,000]
배열 A의 각 요소 정수의 range [-1,000...1,000]

# big-O
time : O(N)
space : O(N)


# point
배열을 P별로 모두 계산하면 O(N^2)
O(N)이면 for문이 한번만 실행되게 구현

## think
두개의 sum이 있다가정
fSum + bSum = result
|fSum - bSum| = 최소값(절대값이므로 0나오면 끝)
> abSum = result -fSum,
> | fSum - result + fSum | = | 2fSum - result |

즉, 앞에서부터 더해가다가 2fSum 이 result에 가장 근사할때

```
stream을 통한 처리보다 for문을 활용해서 처리를 하자.
속도의 차이가 발생한다.
```

## result
Correctness 100%, Performance 100%































