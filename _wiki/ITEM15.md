---
layout  : wiki
title   : 클래스와 멤버의 접근권한을 최소화 하라.
summary : 
date    : 2019-03-13 09:32:26 +0900
updated : 2019-03-14 10:20:12 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 

모든 클래스와 멤버의 접근성을 가능한 한 좁혀야 한다는 것은,
소프트웨어가 올바로 동작하는 한 항상 가장 낮은 접근 수준을 부여해야 한다는 뜻이다.

멤버(필드, 메서드, 중첩 클래스, 중첩인터페이스)에 부여할 수 있는 접근 수준은 4가지다.

- private : 멤버를 선언한 톱래벨 클래스에서만 접근할 수 있다.
- package-private : 멤버가 소속된 패키지 안의 모든 클래스에서 접근할 수 있다. 접근 제한자를 명시하지 않았을때 적용되는 패키지 수준이다.(단, 인터페이스의 멤버는 기본적으로 public 이 적용된다.)
- protected : package-private 의 접근 범위를 포함하며, 이 멤버를 선언한 클래스의 하위클래스에서도 접근 할 수 있다.
- public : 모든곳에서 접근할 수 있다.



```

프로그램 요소의 접근성은 가능한 한 최소한으로 하라.
꼭 필요한 것만 골라 최소한의 public API 를 설계하자.
그 외에는 클래스, 인터페이스, 멤버가 의도치 않게 API로 공개 되는 일이 없도록 해야한다.
public 클래스는 상수용 public static final 필드 외에는 
어떠한 public 필드도 가져서는 안된다. 
public static final 필드가 참조하는 객체가 불변인지 확인하라.

```

