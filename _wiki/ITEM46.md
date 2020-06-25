---
layout  : wiki
title   : 스트림에서는 부작용 없는 함수를 사용하라.
summary : 
date    : 2019-05-15 09:37:00 +0900
updated : 2019-05-15 09:50:10 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  스트림은 그저 또 하나의 API 가 아닌 , 함수형 프로그래밍에 기초한 패러다임이다.
  스트림이 제공하는 표현력, 속도, (상황에 따라서는) 병렬성을 얻으려면 API 는 말할 것도 없고 이 패러다임까지 함께 받아들여야 한다.
  
  
  ```
  // 스트림 패러다임을 이해하지 못한 채 API 만 사용했다 - 따라하지 말것.
Map<String, Long> freq = new HashMap<>();
try(Strean<String> words = new Scanner(file).tokens()){
    words.forEach(word -> {
    freq.merge(word.toLowerCase(), 1L, Long::sum);
            })
}
  ```
  
  
  ```
  // 스트림 을 재대로 활용해 빋노표를 초기화한다.
Map<String, Long> freq ;
try(Strean<String> words = new Scanner(file).tokens()){
    freq = words.collect(groupingBy(String::toLowerCase, counting());
}
  ```
  
  foreach 연산은 스트림 계산 결과를 보고할때만 사용하고, 계산하는 데는 쓰지말자.
  

## 핵심정리
스트림 파이프라인 프로그래밍의 핵심은 부작용 없는 함수 객체에 있다.
스트림뿐 아니라 스트림 관련 객체에 건네지는 모든 함수 객체가 부작용이 없어야 한다. 종단 연상중 foreach 는 스트림이 수행한 계산결과를 보고할때만 이용해야 한다.
계산 자체에는 이용하지 말자. 스트림을 올바로 사용하려면 수집기를 알아둬야한다.
가장 중요한 수직기 팩터리는 toList, toSet, toMap, groupingBy, joining 이다.

