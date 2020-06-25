---
layout  : wiki
title   : 인터페이스는 타입을 정의하는 용도로만 사용하라.
summary : 
date    : 2019-03-25 09:40:18 +0900
updated : 2019-03-25 09:50:45 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 

클래스가 어떤 인터페이스를 구현한다는 것은 자신의 인스턴스로 무엇을 할 수 있는지 클라이언트에게 얘기해주는 것이다.
인터페이스는 오직 이용도로만 사용해야한다.

# 핵심정리
인터페이스는 타입을 정의하는 용도로만 사용해야 한다.
상수 공개용 수단으로 사용하지 말자.

