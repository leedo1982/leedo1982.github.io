---
layout  : wiki
title   : 매개변수가 유효한지 검사하라.
summary : 
date    : 2019-05-20 09:48:30 +0900
updated : 2019-05-20 09:57:14 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  자바 7 에 추가된 jva.util.Objects.requireNonNull 메서드는 유연하고 사용하기도 편하니, 더이상 null 검사를 수동으로 하지 않아도 된다.
  ```
// java null 검사 기능 사용하기
this.strategy  = Objects.requrieNonNull(strategy, "전략 ");

  ```
  반환 값은 그냥 무시하고 필요한 곳 어디든 순수한 null 검사 목적으로 사용해도된다.
  public 이 아닌 메서드라면 단언문(assert) 을 사용해 매개변수 유효성을 검증할 수 있다.
  단언문은 몇가지 면에서 일반적인 유효성 검사와 다르다.
  첫번째, 실패하면 AssertionError 을 던진다.
  두번째, 런타임에 아무런 효과도, 성능저하도 없다.(단, java 실행시 -ea 플래그를 설정하면 다르다.)
  
## 핵심정리
메서드나 생성자를 작성할 때면 그 매개변술들에 어떤 제약이 있을지 생각해야 한다. 그 제약들을 문서화하고 메서드 코드 시작 부분에서 명시적으로 검사해야 한다. 이른 습관을 반드시 기르도록 하자. 그 노력은 유효성 검사가 실제 오류를 처음 걸러낼 때 충분히 보상받을 것이다.

