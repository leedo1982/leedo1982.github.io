---
layout  : wiki
title   : 변경가능성을 최소화하라.
summary : 
date    : 2019-03-18 09:35:18 +0900
updated : 2019-03-18 10:02:18 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리
불변 클래스란 간단히 말해 그 인스턴스의 내부값을 수정할 수 없는 클래스다.  
String, 기분타입의 박싱된 클래스들, BigInteger, BigDecimal이 여기 속한다.  
불변클래스는 갑녀클래스보다 설계하고 구현하고 사용하기 쉬우며, 오류가 생길 여지도 적고 안전한다.  

```
규칙

- 객체의 상태를 변경하는 메서드(뼌경자)를 제공하지 않는다.
- 클래스를 확장할 수 없도록 한다.
- 모든 필드를 final로 선언한다.
- 모든 필드를 private으로 선언한다.
- 자신 외에는 내부의 가변컴포넌트에 접근할 수 없도록 한다.

```

불변 객체는 근본적으로 스레드 안전하여 따로 동기화할 필요 없다.  
불변 객체는 안심하고 공유할 수 있다.  
불변 객체는 자유롭게 공유할 수 있음은 물론, 불변 객체끼리는 내부 데이터를 공유할 수 있다.  
객체를 만들때 다른 불변 객체들을 구성요소로 사용하면 이점이 많다.  
불변 객체는 그 자체로 실패 원자성을 제공한다(실패원자성: 메서드에서 예외가 발생한 후에도 그 객체는 메서드 호출전과 똑같은 유효한 상태여야 한다.)  
불변 클래스에도 단점은 있다. 값이 다르면 반드시 반드시 독립된 객체로 만들어야 한다.  

클래스가 불변임을 보장하려면 자신을 상속하지 못하게 해야한다.  
가장 쉬운 방법은 final 클래스로 선언하는 것이다.  
좀더 유연한 방법은 모든 생성자를 private / package-private 로 만들고, public 정적팩터리를 제공하는 방법이다.  

getter 가 있다고 해서 무조건 setter 을 만들지는 말자  
클래스는 꼭 필요한 경우가 아니라면 불변이어야 한다. 

모든 클래스를 불변으로 만들수는 없다. 불변으로 만들 수 없는 클래스라도 변경할 수 있는 부분을 최소한으로 줄이자.  
생성자는 불변식 설정이 모두 완료된 초기화가 완벽히 끝난 상태의 객체를 생성해야 한다. 


