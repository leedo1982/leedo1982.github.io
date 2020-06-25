---
layout  : wiki
title   : 한정적 와일드 카드를 사용해 API 유연성을 높여라.
summary : 
date    : 2019-04-08 10:02:22 +0900
updated : 2019-04-08 16:07:15 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  매개변수화 타입은 불공변이다. 즉, List<Type1> 은 List<Type2> 하위타입도 상위타입도 아니다.
  
  ```
  // 와일트카드 타입을 사용하지 않은 pushAll 메서드 - 결함이 있다.
  public void pushAll(Iterable<E> src){
      for(E e : src){
        push(e);
      }
  }
  ```
  컴파일이 되지만 완벽하진 않다.
  Stack<Number> 로 선언한 후 pushAll(intVal)을 호출하면, 불공변이기에 에러가 발생한다.
  
  다행히 해결책은 있다. 한정적 와일드카드 타입이라는 특별한 매개변수화 타입을 지원한다.
  pushAll 의 입력 매개변수타입은  'E의 Interable'이 아니라 'E의 하위 타입의 Iterable'이어야 하면,
  와일드 카드 타입 Iterable<? extends E> 가 정확히 이런뜻이다.
  
  ```
  // E 생산자 매개변수에 와일드 카드 타입 적용 
  public void pushAll(Iterable<? extends E> src){
      for(E e : src){
        push(e);
      }
  }
  ```
  메세지는 분명하다. 유연성을 극대화하려면 원소의 생산자나 소비자용 입력 매개변수에 와일드카드 타입을 사용하라.
  한편 입력매개변수가 생산자와 소비자 역할을 동시에 한다면 와일드 카드 타입을 써도 좋을게 없다.
  
  다음 공식을 외워두면 어떤 와일드 가트 타입을 써야하는지 기억하는데 도움이 된다.
  > PECS : producer-extends, consumer-super
  
  
  ```
반환 타입은 한정적 와일드 카드 타입을 사용하면 안된다. 
유연성을 높여주기는 커녕 클라이언트 코드에서도 와일드카드 타입을 써야하기 때문이다.
  ```
  
  
## 핵심정리 
조금 복잡하더라도 와일드카드 타입을 적용하면 API가 훨씬 유연해진다.
그러니 널리 쓰일 라이브러리를 작성한다면 반드시 와일드카드 타입을 적절히 사용해줘야 한다.
PECS 공식을 기억하자. 즉, 생상자는 extends 를 소비자는 super 을 사용한다.
Comparable 과 Comparator 은 모두 소비자라는 사실도 잊지 말자. 
