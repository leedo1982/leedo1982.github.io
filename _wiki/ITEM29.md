---
layout  : wiki
title   : 이왕이면 제네릭 타입으로 만들라.
summary : 
date    : 2019-04-04 09:18:05 +0900
updated : 2019-04-04 09:25:59 +0900
tags    : effective
toc     : true
public  : true
parent  : EFFECTIVE_JAVA
latex   : false
---
* TOC
{:toc}

# 정리
제네릭 타입을 새로 만드는 일은 조금 어렵지만 배워두면 그만한 값어치는 충분히 한다.
타입의 이름은 보통 E 를 사용한다.

첫번째 방식에서는 형변환을 배열 생성시 단 한번만 해주면 되지만, 두번째 방식에서는 배열에서 원소를 읽을 때 마다 해줘야 한다.
따라서 현업에서는 첫번째 방식을 더 선호하면 자주 사용한다.
하지만 E가 Object 가 아닌 한 배열의 런타임 타입이 컴파일타임 타입과 달리 힘 오염을 일으킨다.
힙 오염이 맘에 걸리는 프로그래머는 두번째 방식을 고수하기도 한다. 

## 핵심정리
클랑언트에서 직접 형변환해야 하는 타입보다 제네릭 타입이 더 안전하고 
쓰기 편하다.
그러니 새로운 타입을 설꼐할 때는 형변환 없이도 사용할 수 있도록 하라. 그렇게 하려면 제네릭 타입으로 만들어야할 경우가 많다.
기존 차입 중 제네릭이었어야 하는게 있다면 제네릭타입으로 변경하자. 
기존 클라이언트에는 아무 영햐을 주지 않으면서, 새로운 사용자를 훨씬 편하게 해주는 길이다.

