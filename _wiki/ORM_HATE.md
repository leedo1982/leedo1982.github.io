---
layout  : wiki
title   : OrmHate by Martim Fowler
summary : ormhate
date    : 2018-05-24 16:07:16 +0900
updated : 2018-05-24 17:31:01 +0900
tags    : orm
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# OrmHate(요약)
QCon 컨퍼런스에서 ORM에 대한 경멸을 적어도 45분마다 들었다. 
그러나 나는 ORM Hate에 대하여 부당하다고 생각하기에 반증하고 싶다.

ORM은 복잡하고 관계형 데이터베이스저장소에 대한 부족한 추상화만 제공하는 것으로 요약될 수 있다.
복잡성은 힘든 학습 곡선을 의미하며 종종 기본 데이터베이스와의 순진한 상호작용으로 인해 심하게 수행한다.

이것들은 많은 것이 사실이지만 중요한 맥락을 놓치고 있다. 
ORM 문제는 어렵다. 기본적으로 두개의 다른 표현의 데이타 간의 동기화를 하고 있는 것이다.
하나는 관계형 데이터베이스고 다른 하나는 in-memory이다.
이것을 관계 객체 매핑이라고 부르지만, 객체와는 아무런 관련이 없다.
in-memory와 관계 매칭문제로 언급해야 하는 것이 옳다. 왜냐하면 RDBMS를 in-memory 데이터 구조에 매핑하는 것이 사실이기 때문이다.
in-memory 데이터 구조는 관계형 모델 보다 더 유연하다. 그래서 대부분의 사람들은 in-memory 구조를 더 다양하게 사용하길 원한다. 그러므로 데이터베이스를 위한 관계로 돌아가는 것을 직면하게 된다.

매핑은 다른 쪽에서 매핑하는 양쪽면을 변경할 수 있기에 더욱 복잡하다. 여러사람이 데이터 베이스에 동시에 접속하고 수정할 수 있으므로 더 복잡하다. ORM은 트랜젝션에 의전할 수 없기 때문에 이러한 동시성을 처리해야 한다. 대부분의 경우 트랜젝션 메모리를 사용하는 동안 트랜젝션을 열어둘 수 없다.

많은 사람들이 ORM에 관해 이런 방식으로 무엇인가를 dump하려 한다면, 대안을 말해야 한다고 생각한다.ORM 대신에 무엇을 합니까? 보통 이것을 무시한다. 
기본적으로 두가지 전략으로 요약할 수 있으며, 문제를 다르게 해결하거나 피할 수 있다.
두가지 모두 결함이 있다. 

## 더 나은 해결

비평을 듣고 현대 소프트웨어 개발자가 할 최선의 것은 ORM을 굴리는(roll)일이다.
Hibernate 와 Active Record 같은 도구가 bloatware 되었으므로 자신만의 경량 대안을 찾아야 한다는 것이다.

종종 ORM에 대한 좌절감의 대부분이 부푼 기대감이라고 생각했다.
많은 사람들이 관계형 데이터베이스를 미친 이모처럼 취급했다.
이 세계관에서 그들은 단지 in-memory 데이터 구조를 다루고 
ORM 이 database를 다루도록 원했다.
이러한 사고방식은 작은 응용프로그래밍 및 부하에서 작동할 수 있지만
진행이 어려워지면 곧 사라진다.
본질적으로 ORM은 매핑 문제의 80~90%를 처리할 수 있지만 마지막 청크는 관계형 데이터베이스의 작동
방식을 실제로 이해하는 사람이 주의 깊게 작업해야한다.

ORM은 추상화가 부족하다는 것이 비판을 가지고 온다. 
이것은 사실이지만 반드시 피해야하는 이유는 아니다. 
관계형 데이터베이스를 매핑하는 것은 반복적인 보일러플레이트 코드를 포함한다.
그중 80%를 피할 수있는 프레임워크는 80%에 불과하여도 가치가 있다. 
문제는 100%가 아니라고 생각하는 것이다.

ORM이 해야 할일에 대한 제한된 기대에 대한 결과가 있다.
종종 사람ㄷ르이 ORM을 기쁘게 하기 위해 관계형 모델을 만들기 위해 객체 모델을 손상시키지 않으면 불평한다고들었다. 
실제로 이것은 관계형 데이터베이스를 사용하는 필연적인 결과라 생각한다.
메모리 모델을 보다 관계형으로 만들어야하거나 매핑코드가 볶잡해진다.
객체 관계형 매핑을 단순화하기 위해 더 많은 관계형 도메인을 사용하는 것이 합리적이라고 생각한다.
그렇다고 항상 관계형 모델을 정확히 따라야 한다는 의미는 아니지만 도메인 모델 디자인의 일부로 복잡성을 고려해야 한다.

## 문제예방
매핑 문제를 피하는 두가지 방법이 있다. 
관계형 모델을 메모리에 사용하거나 데이터베이스에서 사용하지 마라.

관계형 모델을 메모리에 사용한다는 것은 기본적으로 응용프로그램을 통해 관계식으로 프로그래밍한다는 것을 의미힌다. 여러면에서 90년대의 CRUD 도구가 주어진 것이다.

디스크에서 관계형 데이터베이스를 사용하지 않는다는 것은 NoSQL 데이터베이스를 사용하라
Nosql은 기술이 매우 중요하다고 생각한다. 
집계와 같이 NoSQL 데이터 모델과 잘 매핑되는 애플리케이션 문자가 있는 경우
완전히 매핑의 nastiness를 피할 수 있다.

[출처:Martin Fower - OrmHate](https://martinfowler.com/bliki/OrmHate.html )
