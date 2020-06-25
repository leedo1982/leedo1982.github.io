---
layout  : wiki
title   : 상속보다는 컴포지션을 사용하라
summary : 
date    : 2019-03-19 09:45:27 +0900
updated : 2019-03-19 10:10:21 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리 
상속 : 클래스가 다른 클래스를 확장하는 구현상속을 의미(인터페이스 상속과는 무관)  
상속은 코드를 재사용하는 강력한 수단이지만, 항상 최선은 아니다.  


메서드호출과 달리 상속은 캡슐화를 깨뜨린다.


기존 클래스를 확장하는 대신, 새로운 클래스를 만들고 private 필드로 기존 클래스의 인스턴스를 참조하게하자  
기존 클래스가 새로운 클래스의 구성요소로 쓰인다는 뜻에서 이러한 설계를 컴포지션 이라한다.  
새 클래스의 인스턴스 메서드들은 (private 필드를 참조하는 ) 기존 클래스의 대응하는 메서드를 호출해 그결과를 반환한다.  
이 방식을 forwarding 이라 하며, 새 클래스이 메서드들을 forwarding method 라 부른다.  

```
// 래퍼 클래스 - 상속대신 컴포지션을 사용했다.
public class InstrumentedSet<E> extends ForwardingSet<E>{
    private int addCount = 0;
    
    public InstrumentedSet(Set<E> s){
        super(s);
    }

    @Override public boolean add(E e){
        addCount++;
        return super.add(e);
    }
    
    @Override public boolean addAll(Collection<? extends E> c){
        addCount += c.size();
        return super.addAll(c);
    }
    
    public int getAddCount(){
        return addCount;
    }
}

// 재사용할 수 있는 전달 클래스

public class ForwardingSet<E> implements Set<E>{
    private final Set<E> s;
    public ForwardingSet(Set<E> s){ this.s = s; }
    
    public void clear() {s.clesr();}
    public boolean contains(Object o){ return s.contains(o) }
    ...
    ...
    ...
    ...
    ...
    ...
}


Set<Instant> time = new InstrumentedSet<>(new TreeSet<>(cmp));
Set<E> time = new InstrumentedSet<>(new HashSet<>(INIT_CAPACITY));


```


래퍼 클래스의 단점은 거의없다. 한가지 래퍼 클래스가 콜백프레임워크와는 어울리지 않는다는 것만 주의하면된다.  

컴포지션 대신 상속을 사용하기로 결정하 전에 마지막으로 자문해야할 질문은  
확장하려는 이 클래스의 API는 정말 아무런 결함이 없는가?  
있다면 새로 생성하는 클래스까지 전파되어도 문제 없는가? 이다.

# 핵심정리
상속은 강력하지만 캡슐화를 해친다는 문제가 있다.  
상속은 상위 클래스와 하위클래스가 순수한 is-a 관계일때만 써야한다.  
is-a 관계일 때도 안심할 수만은 없는게, 하위 클래스의 패키지가 상위클래스와 다르고, 상위클래스가 확장을 고려해 설계되지 않았다면 여전히 문제가 될 수 있다.  
상속의 취약점을 피하려면 상속대신 컴포지션과 전달을 사용하자. 특히 래퍼클래스로 구현할 적당한 인터페이스가 있다면 더욱 그렇다.  
래퍼클래스는 하위 클래스보다 견고하고 강력하다.





