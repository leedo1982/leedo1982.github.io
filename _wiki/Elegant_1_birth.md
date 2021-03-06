---
layout  : wiki
title   : 1장 출생
summary : 
date    : 2021-01-11 10:07:11 +0900
updated : 2021-01-11 10:08:47 +0900
tag     : 
toc     : true
public  : true
parent  : [[2021_BOOK]]
latex   : false
---
* TOC
{:toc}


# 1.1 -er 로 끝나는 이름을 사용하지 마세요 
  클래스는 객체의 팩토리(factory)입니다. new 는 객체의 팩토리를 제어할 수 있는 원시적인 수단입니다.
  클래스는 객체를 만들고, 추적하고, 적절한 시점에 파괴합니다. 저는 여러분이 필요할때 객체를 꺼낼 수 있고, 더이상 필요하지 않는 객체를 반환할 수 있는 객체의 웨어 하우스로 클래스를 바라봤으면 좋겠습니다. 종종 클랫를 객체의 템플렛으로 설명하지만 이설명은 완전히 잘못되었다. 클래스를 객체의 능동적인 관리자로 생각해야한다. 
  클래스 이름을 짓는 잘못된 방법은 클래스의 객체들이 무엇을 하고 있는(doing)지를 살펴본 후 기능(functionality)에 기반해서 이름을 짓는 방법이다. 클래스의 이름은 무엇을 하는지(what he does)가 아니라 무엇인지(what he is)에 기반해야 한다. 다시 말해서, 객체는 그의 역량으로 특정지어져야 합니다. 제가 어떤 사람이니는 키, 몸무게, 피부색과 같은 속성이 아니라, 제가할 수 있는 일(what i can do) 로 설명해야 합니다.\
  객체는 객체의 외부 세계와 내부 세계를 이어주는 연결장치가 아닙니다. 객체는 내부에 캡슐화된 데이터를 다루기 위해 요청할 수 있는 절차의 집합이 아닙니다. 대신 객체는 캡슐화된 데이터의 대표자(represeneative)입니다.
  클래스의 이름이 -er 로 끝난다면, 이 클래스의 인스턴스는 실제로는 객체가 아니라 어떤 데이터를 다루는 절차들의 집합일 뿐입니다. 
  객체는 프로시져의 집합처럼 행동해서는 안됩니다. PrimeNumbers 클래스가 숫자 리스트를 캡슐화하고 있는 동안에는, 외부에서 직접 객체 내부에 포함된 숫자 리스트를 처리하거나 조회하도록 허용해서는 안된다. 대신 PrimeNumbers 는 "지금은 내가 바로 그 목록이야!" 라고 얘기해야 합니다.
  내가 무엇을 하는지와 내가 누구인지는 다릅니다.
  
# 1.2 생성자 하나를 주 생성자로 만드세요 
  2~3개의 매서드와 5~10개의 ctor 을 포함하는 것이 적당하다. 물론 이값을 뒷받침할 만한 과학적인 근거는 없으며, 임의로 정했 뿐입니다. 여기서 핵심은 응집도가 높고 견고한 클래스에는 적은 수의 메서드와 상대적으로 더 많은 수의 ctor 이 존재한다는 점입니다. 
  ctor이 많아질수록 클라이언트가 클래스를 더 유연하게 사용할수 있게 됩니다. 하지만 메서드가 많아질수록 클래스를 사용하기는 더 어려워집니다. 메서드가 많이지면 클래스의 초첨이 흐려지고, 단일책임원칙을 위반하기 때문입니다.
  ctor 의 주된 작업은 제공된 인자를 사용해서 캡슐화하고 있는 프로퍼티를 초기화 하는 일입니다. 이런 초기화 로직을 단 하나의 ctor 에만 위치시키고 primary ctor 이라고 부르길 권장하며, secondary ctor 이라고 부르는 다른 ctor 들이 이 primary ctor 을 호출하도록 만들기 바랍니다. 이 원칙의 핵심은 중복코드를 방지하고, 설계를 더 간결하게 만들기 때문에 유지보수성이 향상된다는 점입니다.
  
# 1.3 생성자에 코드를 넣지 마세요. 
  primary ctor 은 객체 초기화 프로세스를 시작하는 유일한 장소 이기 때문에 제공되는 인자들은 완전해야 합니다. 여기서 완전하다는 말은 어떤 것도 누락하지 않고 중복되는 정보도 없다는 것을 의미합니다. 이시점에서 떠오르는 질문은 인자들을 이용해서 할 수 있는 일은 무엇이고, 할 수 없는 일은 무엇인가 하는 점입니다. 
  경험법칙은 '인자에 돈대지 말라(don't touch the artuments)'는 것입니다.
  ```
  class Cash {
    private int dollars;
    Cash(String dlr) {
        this.dollars = Integer.parseInt(dlr); // 손 대 버렸다. 
    }
  }
  ```
  객체 초기화에는 '코드가 없어야(code-free)'하고 인자를 건드려서는 안됩니다. 대신 필요하다면 인자들을 다른 타입의 객체로 감싸거나 가공하지 않는 형식으로 캡슐화해야 합니다. 다음은 인자로 전달된 텍스트를 '건드리지 않고' 동일한 작업을 수행하도록 수정한 예제입니다.
  ```
  class Cash {
    private Number dollars;
    Cash(String dlr) {
        this.dollars = new StringAsInteger(dlr); 
    }
  }
  class StringAsInteger implements Number {
    private String source;
    StringAsInteger(String src){
        this.source = src;
    }
    int intValue(){
        returnn Integer.parseInt(this.source);
    }
  }
  ```
  실제로 사용하는 시점까지 객체의 변환 작업을 연기합니다. 
  
  진정한 객체지향에서 인스턴스화란 더 작은 객체들을 조합해서(compose) 더 큰 객체를 만드는 것을 의미합니다. 객체를 조합하는 단하나의 이유는 새로운 계약을 준수ㅜ하는 새로운 엔티티(entity)기 필요하기 때문입니다. 
  이 조언을 지지하는 순수하게 기술적인 이유가 몇가지 있습니다. 첫번째 이유는 ctor 에 코드가 없을 경우 성능 최적화가 더 쉽기 때문에 코드의 실행 속도가 더 빨라집니다. 
  생성자에서 코드를 없애면 사용자가 쉽게 제어할 수 있는 투명한 객체를 만들 수ㅜ 있으며, 객체를 이해하고 재사용하기도 쉬워집니다. 객체는 요청을 받을 때만 행동하고 그 전에는 어떤 일도 하지 않습니다. 이 객체들은 좋은 방식으로 매우 '게으릅니다.(lazy)'
  
