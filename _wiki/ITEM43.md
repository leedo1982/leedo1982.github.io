---
layout  : wiki
title   : 람다보다는 메서드 참조를 사용하라.
summary : 
date    : 2019-05-02 09:24:51 +0900
updated : 2019-05-02 10:12:44 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  자바에서는 함수 객체를 심지어 람다보다 더 간결하게 만드는 방법이 있으니, 바로 메서드 참조다. 
  IDE 들은 람다를 메서드 참조로 대체하라고 권할 것이다. 보통은 이득이지만, 항상 그런것은 아니다.
  
## 핵심정리
메서드 참조는 람다의 간단명료한 대안이 될 수 있다. 메서드 참조 쪽이 짧고 명확하다면 메서드 참조를 쓰고, 그렇지 않을때만 람다를 사용하라.

