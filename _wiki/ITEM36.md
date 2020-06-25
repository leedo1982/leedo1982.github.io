---
layout  : wiki
title   : 비트필드 대신 EnumSet 을 사용하라
summary : 
date    : 2019-04-16 09:25:16 +0900
updated : 2019-04-16 09:44:33 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  열거한 값들이 주로(단족이 아닌)집합으로 사용될 경우, 예전에는 각 상수에 서로 다른 2의 거듭제곱 값을 할당한 정수 열겨패턴을 사용해 왔다.
  
  ```
// 구닥다리 기법!
  public class Text {
    public static final int STYLE_BOLD          = 1 << 0 ; // 1
    public static final int STYLE_ITALIC        = 1 << 1 ; // 2
    public static final int STYLE_UNDERLINE     = 1 << 2 ; // 4
    public static final int STYLE_STRUKETHROUGH = 1 << 3 ; // 8
    
    public void applyStyle (int styles){ ... }
  }
  ```
  비트 필드를 사용하면 비트별 연산을 사용해 합집함과 교집합 같은 집합 연산을 효울적으로 수행할 수 있다.
  단점, 비트 필드 값이 그대로 출력되면 단순한 정수 열거 상수를 출력할 때보다 해석하기가 훨씬 어렵다.
  EnumSet 클래스는 열거 타입 상수의 값으로 구셩된 집합을 효과적으로 표현해준다.
  Set 인터페이스를 완벽히 구현하며, 타입 안전하고, 다른 어떤 Set 구현체와도 함께 사용할 수 있다.
  하지만 EnumSet 의 내부는 비트 벡터로 구현되었다.
  
  ```
// EnumSet - 비트필드를 대체하는 현대적 기법
  public class Text {
    public enum Style {STYLE_BOLD, STYLE_ITALIC, STYLE_UNDERLINE, STYLE_STRUKETHROUGH}
        
    // 어떤 Set를 넘겨도 되나, EnumSet 이 가장 좋다. 
    public void applyStyle (Set<Style> styles){ ... }
  }
  ```
  applyStyle 메서드가 EnumSet<Style> 이 아닌 Set<Style> 을 받은 이유는 예상되더라도 이왕이면 인터페이스로 받는게 일반적으로 좋은 습관이다.
  
## 핵심정리 
열거할 수 있는 타입을 한데 모아 집합 형태로 사용한다고 해도 비트 필드를 사용할 이유는 없다. EnumSet 클래스가 비트 필드 수준의 명료함과 성능을 제공하고 아이템 34에서 설명한 열거 타입의 장점까지 선사하기 때문이다. EnumSet 의 유일한 단점이라면 붋변 EnumSet 을 만들 수 없다 는 것이다.
그래도 향후 릴리스에서는 수정되리라 본다. 그때까지는 (명확성과 성능이 조금 희생되지만) Collections.unmodifiableSet 으로 EnumSet 을 감싸 사용할 수 있다.


