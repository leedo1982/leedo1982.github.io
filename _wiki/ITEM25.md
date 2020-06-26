---
layout  : wiki
title   : 톱레벨 클래스는 한 파일에 하나만 담으라.
summary : 
date    : 2019-03-27 09:42:45 +0900
updated : 2019-03-27 09:46:46 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
소스파일 하나에 톱레벨 클래스를 여러 개 선언하더라도 자바 컴파일러는 불평하지 않는다.
하지만 아무런 득이 없을 뿐더러 심각한 위험을 감수해야하는 행위다.

## 핵심정리
교훈은 명확하다. 소스 파일 하나에는 반드시 톱레벨 클래스(혹은 톱레벨 인터페이스)를 하나만 담자.
이 규칙만 따른다면 컴파일러가 한 클래스에 대한 정의를 여러개 만들어 내는 일은 사라진다.
소스파일을 어떤 순서로 컴파일하든 바이너리 파일이나 프로그램의 동작이 달라지는 일은 결코 일어나지 않을 것이다.
  
  
  
  