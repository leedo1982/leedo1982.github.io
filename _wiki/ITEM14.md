---
layout  : wiki
title   : comparable 을 구현할지 고려하라.
summary : comparable
date    : 2019-03-11 10:49:51 +0900
updated : 2019-03-12 16:28:38 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 요점
compareTo 는 object 의 메서드가 아니다.

단순 동치성 비교에 더해 순서까지 비교할 수 있으며, 제네릭하고, equals 와 같다.

알파벳, 숫자, 연대 같이 순서가 명확한 값 클래스를 작성한다면 반드시 comparable 인테페이스를 구현하자.


## 일반규약
이 객체와 주어진 객체의 순서를 비교한다.
이 객체가 주어진 객체보다 작으면 음의 정수를, 같으면 0을 , 크면 양의 정수를 반환한다.
이 객체와 비교할 수 없는 타입의 객체가 주어지면 ClassCastException 을 던진다.

* Comparable 을 구현한 클래스는 모든 x, y에 대해 sgn(x.compareTo(y)) = - sgn(y.compareTo(x))여야 한다. 
* Comparable 을 구현한 클래스는 추이성을 보장해야 한다.
즉, x.compareTo(y)> 0 && y.compareTo(z) 이면, x.compareTo(z) > 0 이다.
* Comparable 을 구현한 클래스는 모든 z에 대해 x.compareTo(y) == 0 이면 sgn(x.compateTo(z) == sgn(y.compareTo(z))) 다. 
* 이번 권고가 필수는 아지만 꼭 지키는게 좋다.
주의 : 이 클래스의 순서는 equals 메서드와 일관되지 않다.




> 주어진 객체를 기준으로 this 객체가 어디에 위치하는 가로 판단하면 쉬울듯하다.



