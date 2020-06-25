---
layout  : wiki
title   : Refactoring
summary : 프로그램의 가치를 높이는 코드 정리기술 
date    : 2018-07-27 09:48:11 +0900
updated : 2019-02-01 15:00:02 +0900
tags    : book
toc     : true
public  : true
parent  : 2018_BOOK_KIST
latex   : false
---
* TOC
{:toc}

# intro
매번 읽다가 잊어버리다가 반복하다.
김범준님의 추천 도서 목록에 있어서 다시 마음을 가다듬고 보게 되었다.

## 코드의 구린내
1. 중복코드(Duplicated Code)  
    똑같은 코드가 두군데 이상 있을때는 그 부분을 하나로 통일하면 프로그램이 개선된다.
2. 장황한 메서드(Long Method)  
    최적의 상태로 장수하는 객체 프로그램을 보면 공통적으로 메서드 길이가 짧다.
    메서드의 기능을 한눈에 알 수 있는 메서드명을 사용하면 그 메서드 안의 코드를 분석하지 않아도 된다.
3. 방대한 클래스(Large Class)  
    기능이 지나치게 많은 클래스에는 보통 엄창난 수의 인스턴스 변수가 들어있다. 
    클래스에 인스턴스 변수가 너무 많으면 중복코드가 반드시 존재하게 마련이다.
    코드 분량이 너무 방대한 클래스에도 중복코드가 많이 있기 마련이다.
4. 과대한 매개변수(Long Parameter List)  
    객체를 사용할 때는 메서드에 필요한 모든 데이터를 전달하는 게 아니라,  그 모든 데이터를 가져올 수 있는 메서드만 전달하면 된다. 
    메서드가 필요로 하는 각종 데이터는 그 메서드가 속한 클래스에 들어 있다.
5. 수정의 산발(Divergent Change)   
    수정의 산발은 한 클래스가 다양한 원인 때문에 다양한 방식으로 자주 수정될때 일어난다.
6. 기능의 산재(Shotgun Surgery)  
    기능의 산재는 수정의 산발과 비슷하지만 정반대다.
    수정할 때마다 여러 클래스에서 수만은 자잘한 부분을 고처야한다면 이 문제를 의심할 수 있다.
7. 잘못된 소속(Feature Envy)  
    객체의 핵심은 데이터와 그 데이터에 사용되는프로세스를 한 데 묶는 기술이라는 점이다.
    잘못 소속된 메서드가 제일 흔히 접근하는 대상은 데이터다.
    소속이 잘못된 메서드는 더 많이 접근하는 클래스에 들어가는 것이 마땅하다.
8. 데이터 뭉치(Data Clumps)    
    두 클래스에 들어있는 인스턴스 변수나 여러 메서드 시그너처에 들어있는 매개변수 처럼, 동일한 3~4 개의 데이터 항목이 여러 위치에 몰려 있는 경우가 많다. 이렇게 몰려 있는 데이터 뭉치는 객체로 만들어야 한다.
9. 강박적 기본 타입사용(Primitive Obsession)  
    객체를 처음 접하는 사람은 보통 숫자와 통화를 연동하는 돈 관련 클래스나 전화번호와 우편번호 같은 특수 문자열 클래스 등
    사소한 작업에 작은 객체를 잘 사용하지 않으려는 경향이 있다.
    이러한 우물안 개구리를 벗어나려면 데이터 값을 객체로 전환 하라.
10. switch 문(Switch Statements)  
    switch 문의 단점은 반드시 중복이 생긴다는 점이다. 이 문제점을 해결할 수 있는 최상의 방법은 재정의를 이용하는 것이다.
11. 평행 상속 계층(Parallel Inheritance Hierarchies)  
    평행 상속계층은 사실 기능의 산재의 특수한 상환이다.
    이 문제점이 있으면 한 클래스의 하위클래스를 만들 때마다 매번 다른 클래스의 하위클래스도 만들아 한다.
    서로 다른 두 상속 계층의 클래스명 접두어가 같으면 이 문제를 의심할 수 있다.
12. 직무유기 클래스(Lazy Class)  
    기존에는 비용 대비 효율성이 좋았으나 리펙토링 실시로 인해 기능이 축소된 클래스, 또는 수정할 계획으로 작성했으나 수정을 실시하지 않아 쓸모없어진 클래스가 바로 이런 직무유기 클래스에 해당한다.
13. 막연한 범용 코드(Speculative Generality)  
    메서드나 클래스가 오직 테스트케이스에만 사용된다면 구린내를 풍기는 유력한 용의자로 막연한 범용 코드를 지목할 수 있다.
14. 임시필드(Temporary Field)  
    임시필드의 구린내는 복잡한 알고리즘에 여러 변수를 사용해야 할 때 풍긴다.
15. 메시지 체인(Message Chains)  
    메시지 채인은 클라이언트가 한 객체에 제 2의 객체를 요청하면, 제 2의 객체가 제 3의 객체를 요청하는 연쇄적요청이 발생하는 문제점을 뜻한다.
16. 과잉 중개 메서드(Middle Man)  
    캡슐화를 통해 내부의 세부적인 처리를 외부에서 볼 수 없게 은페하는 작업도 지나치면 문제가 된다.
17. 지나친 관여(Inappropriate Intimacy)  
    간혹 클래스끼리 관계가 지나치게 밀접한 나머지 서로의 은밀한 부분을 알아내느라 과도란 시간을 낭비하게 될때가 있다.
18. 인터페이스가 다른 대용 클래스(Alternative Classes with Different Interfaces)  
    기능은 같은데 시그너처가 다른 메서드에는 메서드명 변경을 실시해야 한다. 
19. 미흡한 라이브러리 클래스(Incomplete Library Class)  
    많은 이들이 재사용을 객체의 목적이라고 생각하는데, 재사용을 과대평가해서 나온 생각인 것 같다.
    라이브러리클래스에 넣어야 할 메서드가 두개뿐이라면 외래 클래스에 메서드 추가기법을 실시하고, 
    부가기능이 많을 때는 국소적 상속확장 클래스 사용 기법을 실시하자.
20. 데이터 클래스(Data Class)  
    데이터 클래스는 필드와 필드 읽기/쓰기 메서드만 들어있는 클래스다. 
    그런 클래스는 오로지 데이터 보관만 담당하며, 구체적 데이터 조작은 다른 클래스가 수행한다.
    누군가가 제보하기 전에 즉시 필드 캡슐화 기법을 실시해야 한다.
21. 방치된 상속물(Refused Bequest)  
    하위 클래스는 부모 클래스의 메서드와 데이터를 상속받는다. 그런데 그렇게 상속받은 메서드나 데이터가 
    하위 클래스에서 더이상 스이지 않거나 필요 없을 땐 어떻게 될까? 그럴경우 하위클래스는 상속물을 전부 받아
    그 중에서 필요한 것 외에 방치해버리는 문제가 생긴다.
22. 불필요한 주석(Comments)  
    주석이 다 필요 없다거나 주석을 작성하지 말라는 얘기가 아니다.
    주석을 구린내를 감춰주는 탈취제 용도로 쓰일 때가 많기 때문이다.

## 리팩토링 기법
### 메서드 정리
#### 메서드 추출(Extract Method)
> 어떤코드를 그룹으로 묶어도 되겠다고 판단될 땐
> 그 코드를 빼내어 목적을 잘 나타내는 직관적 이름의 메서드로 만들자.

예제: 지역변수 사용
지역변수 문제 중 제일 가벼운 경우는 지역변수가 읽히기만 하고 변경되지 않을때. 이럴땐 지역변수를 그냥 매개변수로 전달하면 된다.

복잡한 경우는 지역변수로의 값 대입이다. 이럴때는 임시변수만 생각하면 된다.
매개변수로의 값 대입이 있을 경우엔 즉시 매개변수로의 값 대입 제거를 실시해야 한다.

```
void printOwing(){
    Enumration e = _orders.elements();
    double outstanding = 0.0;
    
    pringBanner();

    // 외상액 계산
    while(e.hasMoreElements()){
        Order each = (Order) e.nextElement();
        outstanging += each.getAmount();
    }

    printDetails(outstanding);
}

void printDetails(double outstanding){
    System.out.pringln("고객명:"+ _name);
    System.out.pringln("외상액:"+ outstanding);
}

```

```

void printOwing(){
    pringBanner();
    double outstanding = getOutstanding();
}

double getOutstanding(){
    Enumration e = _orders.elements();
    double outstanding = 0.0;

    while(e.hasMoreElements()){
        Order each = (Order) e.nextElement();
        outstanging += each.getAmount();
    }

    return outstanding;
}
```

```
// outstanding 변수는 분명한 초깃값으로만 초기화 되므로, 빼낸 메서드 안에서만 초기화 할 수 있다.
double getOutstanding(){
    Enumration e = _orders.elements();
    double result = 0.0;

    while(e.hasMoreElements()){
        Order each = (Order) e.nextElement();
        result += each.getAmount();
    }

    return result;
}
```

변수를 두개 이상 반환해야 할땐 어쩌지?
1. 최선의 방법은 다른 부분의 코드를 빼내는 것이다. 
    밥아저씨는 하나의 메서드는 하나의 값만 반환하는 것을 좋아햔다.
    각기 다른 값을 하나씩 반환하는 여러개의 메서드를 만드는 방법을 사용한다.

2. 혹시 어떤 방법을 사용해도 메서드 추출이 힘들다면 메서드를 객체로 전환기법을 실시하자.

#### 메서드 내용 직접 삽입(Inline Method)
> 메서드 기능이 너무 단순해서 메서드명만 봐도 너무 뻔할 땐
> 그 메서드의 기능을 호출하는 메서드에 넣어버리고 그 메서드는 삭제하자.

```
int getRating(){
    return (moreThanFiveLateDeliveries()) ? 2 : 1;
}
boolean moreThanFiveLateDeliveries(){
    return _numberOfLateDeliveries > 5;
}

```

```
// after refactor
int getRating(){
    return (_numberOfLateDeliveries > 5) ? 2 : 1;
}
```

#### 임시변수를 멧드 호출로 전환(Replace Temp with Query)
> 수식의 결과를 저장하는 임시변수가 있을 땐,
>   그 수식을 빼내어 메서드로 만든 후, 임시 변수 참조 부분을 전부 수식으로 교체하다.
>       새로 만든 메서드는 다른 메서드에서도 호출 가능하다.

임시변수는 일시적이고 적용이 국소적 범위로 제한된다는 단점이 있다.
임시변수는 자신이 속한 메서드의안에서만 인식되므로, 그 임시변수에 접근하려다 보면 코드는 길어지계 마련이다.
임시변수를 메서드 호출로 수정하면 클래스 안 모든 메서드가 그 정보에 접근할 수 있다. 코드가 훨씬 깔끔해진다.

```
double getPrice(){
    int basePrice = _quantity * _itemPrice;
    double discountFactor;
    if(basePrice > 1000) discountFactor = 0.95;
    else discountFactor = 0.98;

    return basePrice * discountFactor;
}
```

임시변수를 final 로 선언하여 그 임시변수들 값을 한번만 대입받는지 시험해보는 것도 나쁘지 않다.
```
double getPrice(){
    final int basePrice = _quantity * _itemPrice;
    final double discountFactor;
    if(basePrice > 1000) discountFactor = 0.95;
    else discountFactor = 0.98;

    return basePrice * discountFactor;
}
```

after Refactor
```
double getPrice(){
    return basePrice() * discountFactor();
}

private int basePrice(){
    return _quantity * _itemPrice;
}

private double discountFactor(){
    if(basePrice() > 1000) return 0.95;
    else return 0.98;
}

```

#### 직관적 임시변수 사용(Introduce Explaining Variable)
> 사용된 수식이 복잡할 땐
>   수식의 결과나 수식의 일부분을 용도에 부합하는 직관적 이름의 임시변수에 대입하자.

수식이 너무 복잡해져서 이해하기 힘들 수 있다. 이럴때는 임시변수를 사용하면 수식을 더 처리하기 쉽게 쪼갤 수 있다.

임시변수를 생각없이 남용하면 메서드만 복잡해지고 코드를 이해하기 힘들어진다.
그런데 임시변수를 사용하는 이유는 어떤 경우 코드를 약간 덜 지저분하게 만들 수 있기 때문이다.
그러나 임시변수를 사용하고 싶을 때마다 더 좋은 방법이 없는지 부터 생각해본다.

```
double price(){
    // 결제액(price) = 총 구매액(base - price) - 대량구매할인(quantity discount) + 배송비(shipping)
    return _quantity * _itemPrice - 
            Math.max(0, _quantity - 500) * _itemPrice * 0.05 +
            Math.min(_quantity * _itemPrice * 0.1, 100.0)
}
```

```
// 총구매약 치환
double price(){
    // 결제액(price) = 총 구매액(base - price) - 대량구매할인(quantity discount) + 배송비(shipping)
    final double basePrice = _quantity * _itemPrice;
    return  basePrice - 
            Math.max(0, _quantity - 500) * _itemPrice * 0.05 +
            Math.min(basePrice * 0.1, 100.0)
}
```

```
// 대량 구매할인액 치환
double price(){
    // 결제액(price) = 총 구매액(base - price) - 대량구매할인(quantity discount) + 배송비(shipping)
    final double basePrice = _quantity * _itemPrice;
    finaldouble quantityDiacount = Math.max(0, _quantity - 500) * _itemPrice * 0.05 
    return  basePrice - quantityDiacount + Math.min(basePrice * 0.1, 100.0)
}
```

```
// 끝으로 배송료에 대한 임시변수 사용
double price(){
    // 결제액(price) = 총 구매액(base - price) - 대량구매할인(quantity discount) + 배송비(shipping)
    final double basePrice = _quantity * _itemPrice;
    finaldouble quantityDiacount = Math.max(0, _quantity - 500) * _itemPrice * 0.05 
    final double shipping = Math.min(basePrice * 0.1, 100.0);
    return  basePrice - quantityDiacount + shipping;
}
```

보통 직관적 임시변수 사용 보다는 메서드 추출을 적용한다.
메서드로 추출하면 다른 부분에서도 사용할 수 있기 때문이다

#### 임시변수 분리(Split Temporary Variable)
> 루프 변수나 값 누적용 임시변수가 아닌 임시변수에 여러번 값이 대입될 땐 
>   각 대입마다 다름 임시변수를 사용하자

많은 임시변수는 긴 코드의 계산 결가를 나중에 간편히 참조할 수 있게 저장하는 용도로 사용된다.
이러한 변수엔 값이 한번만 대입되어야 한다. 값이 두번 이상 대입된다는 건 그 변수가가 메서드안에서 
여러 용도로사용된다는 반증이다.
임시변수 하나를 두가지 용도로 사용하면 코드를 분석하는 사람에게 혼동을 줄 수 있기 때문이다.

```
double getDistanceTravelled(int time){
    double result;
    double acc = _primaryForce / _mass;
    int primaryTime = Math.min(time, _delay);
    result = 0.5 * acc * primaryTime *primaryTime ;
    int secondaryTime = time - _delay;
    if(secondaryTime > 0){
        double primaryVel = acc * _delay;
        acc = (_primaryForce + _secondaryForce ) / _mass;
        result += primaryVel + secondaryTime + 0.5 * acc * secondaryTime * secondaryTime;
    }
    return result;
}
```
acc 값이 두번 대입된다는 점에 주목.
첫번째 받은 힘으로 생긴 초기 가속도를 저장하고, 
두번째 힘으로 생긴 가속을 더한 값을 저장한다.

```
double getDistanceTravelled(int time){
    double result;
    final double primaryAcc = _primaryForce / _mass;
    int primaryTime = Math.min(time, _delay);
    result = 0.5 * primaryAcc * primaryTime *primaryTime ;
    int secondaryTime = time - _delay;
    
    
# Chapter 07. 객체 간의 기능 이동

