---
layout  : wiki
title   : Override 애너테이션을 일관 되게 사용하라.
summary :
date    : 2019-04-26 09:10:24 +0900
updated : 2019-12-04 15:37:08 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  상위 클래스의 메서드를 재정의하려는 모든 메서드에 @Override 애너테이션을달자. 예외는 한가지 뿐이다. 구체 클래스에서 상위 클래스의 추상메서드를 재정의할 때는 굳이 @Override를 달지 않아도 된다. 
## 핵심정리
 재정의한 모든 메서드에 @Override 애너테이션을 의식적으로 달면 여러분이 실수했을 때 컴파일러가 바로 알려줄것이다. 예외는 한가지 뿐이다. 구체클래스에서 상위 클래스의 추상메서드를 재정의한 경우엔 이 애너테이션을 달지 않아도 된다.(단다고 해서 해로울 것도 없다.)
