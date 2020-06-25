---
layout  : wiki
title   : 타입안전 이종 컨테이너를 고려하라.
summary : 
date    : 2019-04-11 09:33:43 +0900
updated : 2019-04-11 10:03:39 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
  데이터베이스의 행(row)은 임의 개수의 열(column)을 가질수 있다.
  모두 열은 타입 안전하게 이용할 수 있다면 멋질것이다.
  쉬운 방법이 있다. 컨테이너 대신 키를 매개변수화한 다음, 컨테이너에 값을 넣거나 뺄때
  매개변수화한 카를 함께 제공하면 된다. 이렇게하면 제네릭 타입 시스템이 값의 타입이 키와 같음을 보장해줄 것이다.
  이러한 설계 방식을 타입안전이종 컨테이너 패턴이라고 한다.
  
  ```
// 타입 안전 이종 컨테이너 패턴 - API
  public class Favorites{
    public <T> void putFavorite(Class<T> type, T instance);
    public <T> getFavorite<Class<T> type);
  }

// 타입 안전 이종 컨테이너 패턴 - 클라이언트
public static void main(String[] args){
    Favorites f = new Favorites();
    
    f.putFavorite(String.class, "Java");
    f.putFavorite(Integer.class, 0xcafebabe);
    f.putFavorite(Class.class, Favorites.class);
    
    String favoriteString = f.getFavorite(String.class);
    int favoriteInteger = f.getFavorite(Integer.class);
    Class<?> favoriteClass = f.getFavorite(Class.class);
}
  ```
  
  일반적인 맵과 달리 여러가지 타입의 원소를 담을 수 있다. 따라서 Favorites는 타입 안전이종 컨테이너라고 할 만하다.
  
  Class 의 cast 매서드는 형변환 연산자의 동적버전이다.
  
  실체화 불가타입에는 사용할 수 없다. 다시말해 List<String> 와 List<Integer>을 허용하면 둘다 동일해져서 내부는 아수라장이 될것이다.
  이를 완벽히 우회할수 있는 방법은 없다.
  
  슈퍼타입토큰으로 해결하려는 시도도 있다.
  슈퍼타입토큰은 자바업계의 거장인 닐개프터가 고안한 방식으로, 실제로 아주 유용하면 스프링프레임워크에서는 아예 ParameterizedTypeReference 라는 클래스로 미리 구현해 놓았다.
  
## 핵심정리
    컬랙션 API 로 대표되는 일반적인 제네릭 형태에서는 한 컨테이너가 다룰 수 있는 타입매개변수의 수가 고정되어있다. 하지만 컨테이너 자체가 아닌 키를 타입매개변수로 바꾸면 이런 제약이 없는 타입 안전 이종 컨테이너를 만들 수 있다. 타입안전 이종 컨테이너는 Class 를 키로 쓰며, 이런 식으로 쓰이는 Class 객체를 타입 토큰이라 한다. 또한, 직접구현한 키 타입도 쓸 수 있다. 예컨대 데이터베이스의 행(컨테이너)을 표현한 DatabaseRow 타입에는 제네릭 타입인 Column<T>를 키로 사용할 수 있다.
    
