---
layout  : wiki
title   : ordinal 인덱싱 대신 EnumMap 을 사용하라.
summary : 
date    : 2019-04-19 09:20:37 +0900
updated : 2019-04-19 09:32:42 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  정확한 정수값을 사용한다는 것을 여러분이 직접 보증해야 한다. 정수는 열거 타입과 달리 타입 안전하지 않기 때문이다.
  배열은 실질적으로 열거 타입 상수를 값으로 매핑하는 일을 한다. 그러니 Map을 사용할 수도 있을 것이다. 사실 열거타입을 키로 사용하도록 설계한 하주 빠른 Map 구현체가 존재하는데 바로 EnumMap 이 그주인공이다.
  ```
    Map<Plang.LifeCycle, Set<Plant>> plantsByLifeCycle = 
        new EnumMap<>(Plant.LifeCycle.class);
    for(Plant.LifeCycle lc : Plang.LifeCycle.values())
        plantsByLifeCycle.put(lc, new HashSet<>());
    for(Plant p : garden)
        plantsByLifeCycle.get(p.lifeCycle).add(p);
  ```

## 핵심정리
배열의 인덱스를 얻기 위해 ordinal을 쓰는 것은 일반적으로 좋지 않으니, 대신 EnumMap 을 사용하라.
다차원 관계는 EnumMap< ..., EnumMap<...>> 으로 표현하라. "애플리케이션프로그래며는 Enum.ordinal"을 (웬만해서는) 사용하지 말아야 한다."는 일반원칙의 툭수한 사례다.
