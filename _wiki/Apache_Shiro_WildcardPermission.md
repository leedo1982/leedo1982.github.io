---
layout  : wiki
title   : Apache Shiro WildcardPermission
summary : 
date    : 2018-10-10 11:01:48 +0900
updated : 2018-10-11 10:57:13 +0900
tags    : permissions
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# Apache Shiro WildcardPermission
## Understanding Permissions in Apache Shiro
shiro 는 명시적 행위 또는 동작을 정의하는 statement 로 Permission 을 정의한다.  

shiro 는 **"누가(who)" 행동을 수행할 수 있는지 전혀 설명하지 않는다.**

```
Some examples of permissions:
Open a file
View the ‘/user/list’ web page
Print documents
Delete the ‘jsmith’ user
```

"Who"(users)가 "What"(permissions)을 하도록 허용되었는지 정의하는 것은   
user 에게 permissions 을 할당하는 것입니다.   
이는 항상 응용 프로그램의 데이터 모델에 의해 수행되며 응용 프로그램마다 크게 다를 수 있습니다.  

예를 들면, permissions 은 Role 에 그룹화 할 수 있으며, Role 은 하나 이상의 User 와 연결 할 수 있다.  
또는 일부 애플리케이션 사용자의 그룹을 가질 수 있으며 그룹에 Role를 연결할 수 있다.  
이 그룹은 전 이전역절을 통해 해당 그룹의 모든 사용자에게 암시적으로 Role의 permissions 이 부여됨을 의미한다.  

user 에게 permissions 을 부여하는 방법은 여러가지가 있다.  
애플리케이션은 요구사항에 따라 모델링하는 방법이 결정된다.  


### Wildcard Permissions
위의 "Open a file","View the ‘user/list’ web page" 는 모두 유효한 권한 statements 이다.  
그러나 자연언어 문자열을 해석하고 사용자가 해당 동작을 수행할 수 있는지 여부를 결정하는 것은 전산적으로 매우 어려울 것이다.  

따라서 Shiro는 읽기 쉽고 읽기 쉬운 권한 문을 처리하기 쉽도록 WildcardPermission이라고하는 강력하고 직관적인 권한 구문을 제공합니다.  


### Simple Usage
어떤 사람들이 특정 프린터로 인쇄 할 수 있도록 회사의 프린터에 대한 접근을 보고하려는 반면 다른 사용자는 현재 대기열에 있는 작업을 query 할수 있다고 가정해 보자.  

매우 간단한 접근은 "queryPrinter" Permissions 을 부여하는 것이다.  
그러면 당신은 해당 권한이 있는지 확인하면 된다.  
```
subject.isPermitted("queryPrinter")
```
이것은 대부분 아래와 동일하다.
```
subject.isPermitted( new WildcardPermission("queryPrinter") )
```

간단한 Permissions 문자열은 간단한 애플리케이션에서 가능하다.  
그러나 **"printPrinter", "queryPrinter", "managePrinter"등**과 같은
더많은 권한이 요구되면 당신은 "*" 를 사용하여 Permissions 를 부여할 수도 있다.

그러나 이러한 접근을 사용하며,  "all printer permissions" 라는 것을 말할 방법이 없다. 
이러한 이유로 WildcardPermission 은 여러 수준의 permissions 을 지원한다.

### Multiple Parts
WildcardPermission 은 여러 레벨 및 부분의 컨셉을 지원합니다.   
예를 들면, user 에게 permission 을 부여함 으로 이전의 간단한 예제를 재구성할 수 있다.
```
printer:query
```
콜론(:) 은 permission 문자열에서 다음 part 를 구분하는 특수 문자입니다.

이 예제에서 **첫번째 부분은 작업중 인 도메인** 이고, **두번째 부분은 수행되는 작업**이다.
다른 예제는 다음과 같이 변경됩니다.
```
printer:print
printer:manage
```

부분의 수에는 제한이 없으므로, 애플리케이션에서 사용하기 위한방법의 관점에서 당신의 상상력에 달려 있다.


#### Multiple Values
각 부분은 여러 값을 포함할 수 있다. 따라서 user 에게"printer : print", "printer : query" 두개의 permission 을 부여하는 대신 간단히 다음과 같이 부여할 수 있다.
```
printer:print,query
```

print 와 query 의 행위를 허가 하였으므로, 다음과 같이 체크할 수 있다.
```
subject.isPermitted("printer:query")
```

#### All Values
특정 파트의 모든 값을 user 에게 부여하려면 어떻게 해야하나?
수동으로 모든값을 나열하는 것보다, 와일드 카드 문자를 기반으로 작업을 수행할 수 있다.
```
printer:query,print,manage
```

간단히 다음과 같이 표현한다.
```
printer:*
```

마지막으로 와일드 카드 사용 권한 문자열의 일부에서 와일드 카드 토큰을 사용할 수도 있다.
예를 들어 user 에게 프린터가 아닌 모든 도메인에서 "view" 에 대해 permission 을 부여하려면
다음을 같이 부여할 수 있다.
```
*:view
```

### Instance-Level Access Control
WildcardPermission 의 또 다른 일반적인 사용법은 인스턴스 수준의 액세스 제어 목록을 모델링 하는 것이다.
이 시나리오에서는2부분을 사용한다.
첫번쩨, 도메인(domain) 
두번째, 작업(action)
세번째, 실행되는 인스턴스(instance)

```
printer:query:lp7200
printer:print:epsoncolor
```
첫 번째는 ID lp7200을 사용하여 프린터를 쿼리하는 동작을 정의합니다.   
두 번째 사용 권한은 ID가 epsoncolor 인 프린터로 인쇄 할 동작을 정의합니다.   
사용자에게 이러한 사용 권한을 부여하면 특정 인스턴스에서 특정 동작을 수행 할 수 있습니다.  
그런 다음 코드를 체크인 할 수 있습니다.  

```
if ( SecurityUtils.getSubject().isPermitted("printer:query:lp7200") {
    // Return the current jobs on printer lp7200 }
}
```
이는 사용 권한을 표현하는 매우 강력한 방법입니다.  
그러나 새로운 프린터를 시스템에 추가 할 때 특히 모든 프린터에 대해 여러 인스턴스 ID를 정의하지 않아도 확장 성이 좋지 않습니다.  
대신 와일드 카드를 사용할 수 있습니다.  

```
printer:print:*
```
이것은 새로운 프린터를 다루기 때문에 규모가 커집니다.
모든 프린터에서 모든잡업에 대한 엑세스를 허용 할 수도 있습니다.
```
printer:*:*
```

또는 단일 프린터에 대한 모든 작업을 할수 있다.
```
printer:*:lp7200
```

특정행동 조차도 표현할 수 있다.
```
printer:query,print:lp7200
```
'*'와일드 카드 및 ','하위 부분 구분 기호는 권한의 모든 부분에서 사용할 수 있습니다.

#### Missing Parts
사용 권한 할당에 대해주의해야 할 마지막 사항 : 누락 된 부분은 사용자가 해당 부분에 해당하는 모든 값에 액세스 할 수 있음을 의미합니다. 다른 말로,
```
printer:print
```
아래와 동일하다.
```
printer:print:*
```
---
```
printer
```
아래와 동일하다.
```
printer:*:*
```
---


그러나 문자열 끝 부분 만 남겨 둘 수 있다.
```
printer:lp7200
```
아래와 같지 않다. 
```
printer:*:lp7200
```

### Checking Permissions
사용 권한 할당은 편의성과 확장 성을 위해 와일드 카드 구조 ( "printer : print : *"= 모든 프린터로 인쇄)를 사용하지만 런타임의 권한 확인은 항상 가능한 가장 구체적인 사용 권한 문자열을 기반으로해야합니다.

```
if ( SecurityUtils.getSubject().isPermitted("printer:print:lp7200") ) {
    //print the document to the lp7200 printer }
}
```
이 검사는 매우 구체적이며 사용자가 그 순간에 무엇을 시도하는지 명시 적으로 반영합니다.

그러나 다음은 런타임 검사에 덜 이상적입니다.
```
if ( SecurityUtils.getSubject().isPermitted("printer:print") ) {
    //print the document }
}
```

### Implication, not Equality
런타임 권한 검사가 가능한 한 구체적이어야하지만 권한 지정이 좀 더 일반적인 이유는 무엇입니까? 이는 권한 검사가 함축 논리에 의해 평가되므로 평등 검사가 아니기 때문입니다.

즉, <strong>사용자에게 user : * 권한이 할당 된 경우   </strong>  
<strong>이는 **사용자가 user:view 작업을 수행 할 수 있음**을 의미합니다.  </strong>  
<strong>"user : *"  문자열은 "user : view"와 분명히 같지 않지만 전자는 후자를 의미합니다. </strong>  
"user : *"는 "user : view"로 정의 된 기능의 수퍼 셋을 설명합니다.  





---
출처 : [https://shiro.apache.org/permissions.html ](https://shiro.apache.org/permissions.html )
