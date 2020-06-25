---
layout  : wiki
title   : 메서드 시그니처를 신중히 설계하라
summary : 
date    : 2019-05-22 09:23:35 +0900
updated : 2019-05-22 09:31:58 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  개별 아이템으로 두기 애매함 API 설계 요령
  1. 메서드 이름을 신중히 짓자. : 긴이름은 피하자.
  2. 편의 메서드를 너무 많이 만들지 말자. : 확신이 서지 않으면 만들지 말자.
  3. 매개변수 목록은 짧게 유지하자.:같은 타입의 매개변수 여러개가 연달아 나오는 경우가 특히 해롭다.
  4. 매개변수의 타입으로는 클래스보다는 인터페이스가 더 낫다.
  5. boolean 보다는 원소 2개 짜리 열거 타입이 낫자.
