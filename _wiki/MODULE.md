---
layout  : wiki
title   : MODULE
summary : 모듈, 패키지라고도 함
date    : 2018-05-14 13:10:09 +0900
updated : 2018-05-14 13:46:47 +0900
tags    : DDD
toc     : true
public  : true
parent  : Domain_Driven_Design
latex   : false
---
* TOC
{:toc}

# module
모든 사람들이 MODULE 을 사용하지만 그중에서 MODULE 을 하나의 완전한 자격을 갖춘 모델 요소로 여기는 사람은 거의 없다. 코드는 기술적 아키텍처에서 개발자에게 할당된 작업까지 온갖범주의 것으로 나뉜다. 그러나 리펙터링을 많이 하는 개발자도 프로젝트 초기에 생각해낸 모둘에 만족하는 경향이 있다.
MODULE 간에는 결합도가 낮아야 하고, MODULE 의 내부 응집도가 높아야 한다는 것은 두말하면 잔소리이다. 결합도와 응집도에 대한 설명은 그것을 기술적인 측정 기준처럼 들리게해서 연관관계와 상호작용의 배분방법에 근거해 결합도와 응집도의 정도를 기계적으로 판단하게 만든다. **그러나 MODULE 로 쪼개지는 것은 코드가 아닌 바로 개념이다.** 어떤 사람이 한번에 생각해낼 수있는 양에는 한계가 있으며, 일관성이 없는 단편적인 생각은 획인적인 생각을 섞어 놓은 것처럼 이해하기 어렵다.

도메인 주도 설계의 다른 모든것들과 마찬가지로 MODULE 도 하나의 의사소통 메커니즘이다.
우리는 분할되는 객체의 의미에 따라 MODULE 을 선택해야한다.

시스템의 내력을 말해주는 MODULE 을 골라 일련의 응집력 있는 개념들을 해당 MODULE 에 담아라.
이렇게 하면 종종 모듈간의 결합도가 낮아지기도 하는데, 그렇게 되지 않는다면 모델을 변경해서 얽혀 있는 개념을 풀어낼 방법을 찾아보거나, 아니면 의미 있는 방식으로 모델의 각 요소를 맺어줄, MODULE 의 기준이 될 법한 것 중 미처 못보고 지나친 개념을 찾아보라. 서로 독립적으로 이해하고 논리적으로 추론할 수 있다는 의미에서 낮은 결합도가 달성되도록 노력하라. 높은 수준의 도메인 개념에 따라 모델이 분리되고 그것에 대응되는 코드도 분리될 때까지 모델을 정제하라.
UBIQUITOUS_LANGUAGE 를 구성하는 것으로 MODULE 의 이름을 부여하라. MODULE 과 MODULE 의 이름은 도메인에 통찰력을 줄 수 있어야 한다.

## 기민한 MODULE
MODULE 은 모델과 함께 발전해야 한다. MODULE 을 선택할 때 초기에 한 불가피한 실수로 결합도가 높아지면 리펙토링을 수행하기 어려워진다. 리팩토링을 자주 수행하지 않는다면 상황은 점점 나빠질 것이다. 고통을 꾹 참고 경험을 바탕으로 문제가 있는 부분의 모듈을 재조직해야만 문제를 해결할 수 있다.

## 인프라스트럭처 주도 패키지화의 함정
```text 
기술적인 정교함이 주도하는 패키지화 계획에는 두가지 비용이 따른다.
1. 프레임워트의 분할 관례 탓에 개념적 객체를 구현하는 요소가 서로 떨어져 있으면 더는 코드에서 모델이 드러나지 않는다. 
2. 머릿속으로 다시 합칠 수 있는 만큼밖에 분할돼 있지 않은데 프레임워크에서 그렇게 분할된 결과를 모조리 사용해 버리면, 도메인 개발자들은 모델을 의미있는 조각으로 나누는 능력을 잃어버리게 된다.

```

여러 서버에서 코드를 분산하는 것이 실제로 의도했던 바가 아니라면 동일한 객체는 아니라도 하나의 개념적 객체를 구현하는 코드는 모두 같은 MODULE 에 둬야 한다.
패키지화를 바탕으로 다른 코드로부터 도메인 계층을 분리하라. 그렇게 할 수 없다면 가능한한 도메인 개발자가 자신의 모델과 설계 의사결정을 지원하는 형태로 도메인 객체를 자유로이 패키지화할 수 있게 하라.






