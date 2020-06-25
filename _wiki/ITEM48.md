---
layout  : wiki
title   : 스트림 병렬화는 주의해서 적용하라.
summary : 
date    : 2019-05-17 09:50:58 +0900
updated : 2019-05-17 10:28:47 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  자바 7 는 고성능 병렬분ㄴ해 프레임워크인 포크-조인 패키지를 추가했다. 그리고 자바 8 부터는 parallel 메서드만 호출하면 파이프라인을 병렬 실행할 수 있는 스트림을 지원했다. 하지만 이를 올바르고 빠르게 작성하는 일은 여전히 어려운 작업니다. 
  환경이 아무리 좋더라도 데이터 소스가 Stream.Iterate 거나 중간 연산으로 limit 를 쓰면 파이프라인 병렬화로는 성능 개선을 기대할 수 없다.
  대체로 스트림의 소스가 ArrayList, HashMap, HashSet, ConcurrentHashMap의 인스턴스거나 배열 int 범위 long 번위일때 병렬화의 효과가 가장 좋다.
  위 자료구조들의 공통점은 참조 지역성이 뛰어나다는 것이다.
  스트림을 잘못 병렬화 하면 성능이 나빠질 뿐만 아니라 결과 자체가 잘못되거나 예상 못한 동작이 발생할 수 있다.
  조건이 잘 갖춰지면 parallel 메서드 호출 하나로 거의 프로세서 코어 수에 비례하는 성능 향상을 만끽할 수 있다.
  
