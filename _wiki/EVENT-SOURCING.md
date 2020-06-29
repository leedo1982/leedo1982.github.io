---
layout  : wiki
title   : EVENT Sourcing - 이벤트 소싱
summary : Event Sourcing과 Fintech Platform Webinar
date    : 2020-06-27 10:08:23 +0900
updated : 2020-06-29 16:00:56 +0900
tag     : eventsourcing 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 개요
webinar(https://www.youtube.com/watch?v=1QH6rKSGvY4&feature=youtu.be) 를 들으면서 정리한 내용입니다.

# Agenda
1. 이벤트 소싱소개
2. 이벤트 소싱과 핀테크
3. 이벤트 소싱과 Audit Trail
4. 이벤트 소싱과 Big Data
5. 이벤트 소싱과 기존 방식의 차이점
6. 이벤트 소싱의 단점

## 1. 이벤트 소싱 소개

- 최신상태를 중심으로하는 설계
- event sourcing 을통한 설계
- 상태 저장을 최소한으로 하는 stateless 한 플랫폼 설계
- etc....

이벤트 소싱은 역시 다양한 옵션중 한가지일뿐

### 데이터는 무엇?
데이터 = 이벤트 혹은 현재상태로 나눔  
![title](https://user-images.githubusercontent.com/12313132/85939991-bac3b180-b954-11ea-9bb5-4e0490c705b0.png){: width="700" height="400"}


기존설계방식은 현재상태에 집중한다.  
이벤트 소싱 방식은 이벤트에 집중한다.  
이벤트는 이미 발생한것을 기록하므로 항상 과거형이다.

    같은 원리가 정용되는 형태 = git , 회계원리 장부

## 2. 이벤트 소싱과 핀테크
### 핀테크 스타트업의 딜레마: 스타트업으로써의 제약 + 금융당국의 규제
실수에 대한 포용성이 매우 낮은편
설립 단계부터 안정성과 보안에 대한 고민을 필요로함
    금융 당국의 다양한 요구사항들
일반적인 스타트업의 접근방식은 위험함(Fake it till you make, Move fast and break things)

![title](https://user-images.githubusercontent.com/12313132/85940020-ecd51380-b954-11ea-8086-e248cdf76b39.png){: width="700" height="400"}
![title](https://user-images.githubusercontent.com/12313132/85940026-f3fc2180-b954-11ea-8640-28940e898e2f.png){: width="700" height="400"}

## 3. 이벤트 소싱과 Audit Trail
감사를 위해 입력된 데이터가 어떤 변환과정을 거쳐 출력되는지의 과정을 기록하여 추적하는 방법

### 종류
1. 시스템 레벨 감사 추적
2. 어플리케이션레벨 감사추척
    a. 보안 사고 방지 / 대응
    b. 어플리케이션의 결함 발견
3. 유저레발 감사추적
    a. 플랫폼 이용자의 audit trail 
    b. 책임여부를 따지거나 이상행동 탐지
4. 물리적 접근의 감사추적


### audit trail 시스템
거의 대부분의 은행들 자산운용사들은 시스템을 별도로 갖추고 있다.
상용 어플레케이션도 다양하게 존재


## 4. 이벤트 소싱과 Big Data
### 이벤트소싱 샘플 데이터
![title](https://user-images.githubusercontent.com/12313132/85940008-d9c24380-b954-11ea-90d7-08fbe2535d26.png){: width="700" height="400"}

### 이벤트소싱 저널(카산드라)
![title](https://user-images.githubusercontent.com/12313132/85940030-f6f71200-b954-11ea-91d4-a1b354b7e0fe.png){: width="700" height="400"}

big data 가 곧 무기인 시대   
data driven 플랫폼이 곧 경재력이다.  
현재 상태를 이루는 모든 이벤트들이 기록되어 있기 때문에 방대한 data 를 보유 할 수 밖에 없는구조  
![title](https://user-images.githubusercontent.com/12313132/85940031-f8283f00-b954-11ea-9258-dead81caea6d.png){: width="700" height="400"}

이벤트의 가치 = 상황에 따라 의미있는 정보를 유추할 수 있는 단서가 됨
![title](https://user-images.githubusercontent.com/12313132/85940033-f9f20280-b954-11ea-833b-181f24a9e587.png){: width="700" height="400"}


## 5. 이벤트 소싱과 기존 방식의 차이점
### 기존의 전동적인 CRUD 방식
최신상태에 직접 CRUD

### 차이
이벤트 소싱 시스템이 아니여도 당연히 기본적인 이벤트 정보를 저장함
BUT 이벤트를 중심으로 설계하는 것과는 다름

![title](https://user-images.githubusercontent.com/12313132/85940035-fbbbc600-b954-11ea-81ae-673dd3ef76cf.png){: width="700" height="400"}

## 6. 이벤트 소싱의 단점
Tech에서는 모든 것이 trade off!
많은 장점이 만큼 다양한 한계점 / 어려움이 공존한다.

![title](https://user-images.githubusercontent.com/12313132/85940036-fbbbc600-b954-11ea-84af-9a67ac36c722.png){: width="700" height="400"}


### 높은 개발난이도
이벤트 중심의 사고는 비지니스 도메인에 대한 깊은 이해가 필요
시스템이 많은 양의 데이터를 다룰수 있어야 함.

### 시스템 복잡성
주로 마이크로서비스 구조로 구현도는 이벤트 소싱
분산환경과 이벤트 소싱이 함께 존재하면서 발생하는 다양한 제약들
이러한 제약들을 극복하기 위해 다양한 테크닉이 존재

### 오류발생시 교정의 어려움

### 개인정보 삭제 요청
고객이 본인 PII 삭제 요청을 할 경우  
이벤트를 불변의 fact로 다르는 이벤트 소싱의 특징  
고객의 개인정보 관련한 이벤트들을 모두다 완별하게 삭제하거나 익명화 해야함


# 참조
[Event Sourcing과 Fintech Platform](https://www.youtube.com/watch?v=1QH6rKSGvY4&feature=youtu.be)
