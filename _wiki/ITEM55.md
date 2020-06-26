---
layout  : wiki
title   : 옵셔녈 반환은 신중히 하라.
summary : 
date    : 2019-05-28 09:20:19 +0900
updated : 2019-05-28 09:31:50 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  옵셔널을 반환하는 메서드에서는 절대 null 을 반환하지 말자.
  null 을 반환하거나 예외를 던지는 대신 옵셔널 반환을 선택해야하는 기준은 무엇인가? 옵셔널은 검사예외와 취지가 비슷하다. 즉, 반환값이 없을 수도 있음을 API 사용자에게 명확히 알려준다.
  
  자바8 에서는 다음과 같이 구현할 수 있다.
  ```
  streamOfOptionals.filter(Optional::isPresent).map(Optional::get)
  ```
  
  컬렉션, 스트림, 배열, 옵셔널 같은 컨테이너 타입은 옵셔널로 감싸면 안된다.
  
  옵셔널을 사용하는 기본 규칙은. 결과가 없을 수 있으며, 클라이언트가 이상황을 특별하게 처리해야 한다면 Optional<T>를 반환한다.
  
  옵셔널을 컬렉션의 키, 값, 원소나 배열의 원소로 사용하는게 적절한 상황은 거의 없다.
  
## 핵심정리
값을 반환하지 못할 가능성이 있고, 호출할 때마다 반환값이 없을 가능성을 염두에 둬야 하는 메서드라면 옵셔널을 반환해야 할 상황일 수 있다. 하지만, 옵셔널 반환에는 성능 저하가 뒤따르니, 성능에 민감한 메서드라면 null 을 반환하거나 예외를 던지는 편이 나올수 있다. 그리고 옵셔널을 반환값 이외의 용도로 쓰는 경우는 매우 드믈다.
