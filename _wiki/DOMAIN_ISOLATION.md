---
layout  : wiki
title   : 04.도메인의 격리 
summary : 
date    : 2019-12-05 10:08:16 +0900
updated : 2020-01-03 06:39:05 +0900
tags    : DDD
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 계층형 아키텍처(LAYERED ARCHUTECTURE)
```

객체지향 프로그램에서는 종종 사용자 인터페이스와 데이터 베이스 기타 보조적인 성격의
코드를 비지니스 객체안에 직접 작성하기도 한다. 부가적인 업붐로직은 UI 위젯과
데이터베이스 스크립트에 들어간다. 이런 일이 발생하는 까닭은 단기적으로는 이렇게 하는
것이 뭔가 동작하게 하는 가장 쉬운 방법이기 때문이다.

```

```
도메인에 관련된  코드가 상당한 양의 도메인과 관련이 없는 다른 코드를 통해 널리 확산될 경우
도메인에 관련된 코드를 확인하고 추론하기가 굉장히 힘들어진다. UI를 표면적으로 변경하는 것이
실질적으로 업무 로직을 변경하는 것으로 이어질 수 있다. 업무 규칙을 변경하고자 UI코드나
데이터베이스 코드, 또는 다른 프로그램 요소를 세심하게 추적해야 할지도 모른다. 응집력 있고
모델 주도적인 객체를 구현하는 것이 비현실적인 이야기가 돼버리고 자동화 테스트가 어려워진다.
기술과 로직이 모두 각활동에 포함돼 있다면 프로그램을 매우 단순하게 유지해야 하며, 그렇지 않으면
프로그램을 이해하기가 불가능해진다.

```


```
사용자 인터페이스 : 사용자에게 정보를 보여주고 사용자의 명령을 해석하는 일을 책임잔다.
응용계층 : 소프트웨어가 수행할 작업을 정의하고 표현력 있는 도메인 객체가 문제를 해결하게 한다. 
         이계층에서 책임지는 작업은 업무상 중요하거나 다른 시스템의 응용계층과 상호작용
         이계층은 앏게 유지된다. 업무규칙이나 지식이 포함되지 않는다.
도메인 계층 : 업무걔년과 업무 상황에 관한 정보, 업무규칙을 표현하는 일을 책임진다. 핵심계층
인프라스트럭처 계층 : 상위 계층을 지원하는 일반화된 기술적 기능을 제공한다.
```

```
복잡한 프로그램을 여러 개의 계층으로 나눠라. 응직력 있고 오직 아래에 위치한 계층에만
의존하는 각 계층에서 설계를 발전시켜라. 표준 아키텍처 패턴에 따라 상위 계층과의 결합을
느스하게 유지하라. 도메인 모델과 관련된 코드는 모두 한계층에 모으고 사용자 인터페이스 코드나
애플리케이션코드, 인프라스트럭처 코드와 격리하라. 도메인 객체(표현이나 저장, 애플리케이션 작업을
관리하는 등의 책임에서 자유로)는 도메인 모델을 표현하는 것에만 집중할 수 있다.
이로써 모델은 진화를 거듭해 본질적인 업무 지식을 포착해서 해당 업무 지식이 효과를 발휘할 수 있을
만큼 풍부하고 명확해 질것이다.
```

SMART UI(지능형 UI)"안티패턴"

```
프로젝트 경험이 많지 않은 팀에서 단순한 프로젝트에 LAYERED ARCHITECTURE 와 함께 
MODEL_DRUVEN DESIGN 을 적용하기로 결정했다면 그 프로젝트 팀음 매우 험악한 학습 곡선에
직면할 것이다. 팀원들은 복잡한 신기술에 통달해야하고 객체모델링을 학습하는 과정에서
차질을 빚게 된것이다. 인프로스트럭처 계층을 관리하는 데 따르는 부하 탓에 매우 단순한
과업을 수행하는 데조차 시간이 오래 걸린다. 단순한 프로젝트에는 짧은 일정과 정닥한
기대만 따른다. 결국 팀에 할당된 과업을 완수하기도 전에 그러한 접근법이 가져다 줄
흥미진진한 가능성을 거의 보여주지 못한채 프로젝트는 취소될 것이다.

설령 그팀에 시간을 주더라도 전문자의 도움없이는 팀원들이 기술을 능숙하게 익히지 
못할 가능성이 높다. 그리고 결국 그 팀이 이러한 문제를 극복하더라도 하나의 단순한
시스템만이 만들어질 적이다. 풍부한 기능을 요청한 적도 없는데 말이다.

모든 업무로직을 사용자 인터페이스에 넣어라. 애플리케이션을 작은 기능으로 작은 기능으로 잘게 나누고,
나눈 기능을 각기 분리된 사용자 인터페이스로 구현해서 업무규칙을 분리된 사용자 인터페이스에
들어가게 하라. 관계형 데이터베이스를 데이터의 공유 저장소로 사용하고 이용가능한 최대한 자동화된
UI 구축 도구와 시각적인 프로그래밍 도구를 사용하다.

아키텍처에 응집력 있는 도메인 설계가 시스텐의 다른 부분과 느슨하게 결합될 수 있게 도메인
관련 코드를 격리한다면 아마 그러한 아키텍처는 도메인 주도 설계를 지원할 수 있을 것이다.

```



