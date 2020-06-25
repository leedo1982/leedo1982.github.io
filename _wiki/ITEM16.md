---
layout  : wiki
title   : public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라.

summary : 
date    : 2019-03-15 09:39:14 +0900
updated : 2019-03-15 09:44:16 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리
패키지 바깥에서 접근할 수 있는 클래스라면 접근자를 제공함으로써 클래스 내부 표현방식을 언제든 바꿀 수 있는 유연성을 얻을수 있다.
하지만 private-package 클래스 혹은 private 중첩 클래스라면 데이터 필드를 노출한다 해도 하등의 문제가 없다.


```
public 클래스는 절대 가변필드를 직접 노출해서는 안된다.
불변 필드라면 노출해도 덜 위험하지만 완전히 안심할 수는 없다.
하지만 package-private 클래스나 private 중첩 클래스에서는 종종 필드를 노출하는 편이 나을 때도 있다. 
```
