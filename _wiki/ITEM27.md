---
layout  : wiki
title   : 비검사 경고를 제거하라.
summary : 
date    : 2019-03-29 09:54:02 +0900
updated : 2019-03-29 10:01:29 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  컴파일러가 알려준 타입 매개변수를 명시하지 않고, 자바 7부터 지원하는 다이아몬드 연산자(<>)만으로 해결할 수 있다.
  할수 있는 한 모든 비검사 경고를 제거하라.  모두제거한다면 그 코드는 타입 안전성을 보장한다.
  경고를 제거할 수 없지만 타입이 안전하다고 확신한다면 @SuppressWarnings("unchecked") 를 달아 경고를 숨기자
  @SuppressWarnings("unchecked")는 항상 가능한 한 좁은 범위에 적용하자.
  @SuppressWarnings("unchecked") 사용할 때면 그 경고를 무시해도 안전한 이유를 항상 주석으로 남겨야 한다.
  
## 핵심정리 
비검사 경고는 중요하니 무시하지 말자. 모든 비검사 경고는 런타입에 ClasscastException 을 일으킬 수 있는 잠재적 가능성을 뜻하니 최선을 다해 제거하라.
경고를 없앨 방법을 찾지 못하겠다면, 그 코드가 타입안전함을 증명하고 가능한 한 범위를 좁혀 사용하고 경고를 숨겨라. 그런 다음 경고를 숨기기로한 근거를 주석으로 남겨라. 

