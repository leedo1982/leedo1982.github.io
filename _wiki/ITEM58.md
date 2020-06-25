---
layout  : wiki
title   : 전통정인 for 문 보다는 for-each 문을 사용하라.
summary : 
date    : 2019-05-30 09:31:51 +0900
updated : 2019-05-30 09:36:51 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  안타깝게도 for-each 를 사용할 수 없는 상황이 세가지 존재한다.
  - 파과적인 필터링
  - 변형 : 리스트나 배열을 순회하면서 그 원소의 값 일부 혹은 전체를 교체
  - 병렬 반복 : 여러 컬렉션을 병렬로 순회해야 하는 경우
  
## 핵심정리
전통적인 for 문과 비교했을때 for-each 문은 명료하고, 유연하고, 버그를 예방해준다.성능 저하도 없다. 가능한 모든 곳에서 for 문이 아닌 for-each 문을 사용하자.

