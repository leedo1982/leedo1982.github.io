---
layout  : wiki
title   : ordinal 메서드 대신 인스턴스 필드를 사용하라.
summary : 
date    : 2019-04-15 09:57:53 +0900
updated : 2019-04-15 10:01:50 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  대부분의 열거 타입 상수는 자연스럽게 하나의 정숫값에 대응된다.
  해당 상수가 그 열거 타입에서 몇변째 위치인지를 반환하는 oridnal 이라는 메서드를 제공한다.
  
  열거 타입상수에 연결된 값은 oridnal 메서드로 얻지 말고 인스턴스로 저장하다.
  
  ```
Enum Api 문서를 보면 ordinal에 대해 이렇게 쓰여있다.
"대부분의 프로그래머는 이메서드를 쓸일이 없다. 이 메서드는 EnumSet 과 EnumMap 같이 열거 타입기반의 범용 자료구조에 쓸 목적으로 설계 되었다." 
  ```
  따라서 이런 용도가 아니라면 ordinal 메서드는 절대 사용하지 말자.
  
  
