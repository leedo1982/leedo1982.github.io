---
layout  : wiki
title   : Spring-data-rest 를 사용해보자
summary : rest를 더 쉽고 빠르게
date    : 2018-05-15 10:34:44 +0900
updated : 2018-05-16 17:39:03 +0900
tags    : spring, rest
toc     : true
public  : false
parent  : Tech
latex   : false
---
* TOC
{:toc}

# introduce
rest api를 쉽게 만들기 위한 기술
repository 인터페이스의 정의만으로 Rest api를 제공한다.

## 특징
* Repository 인터페이스 정의만으로 REST API 제공
* Query Method : 메소드 선언으로 검색 API 지원
* Projection : 데이터 표현 방식을 다양하게 정의/표현 가능
* HATEOS : MetaData 표현(Model, Link, Resource)

## 우리도 적용해보자..

#### 문제점이 무엇이 있을까?!(논의 해보자)
* 우리는 dto에 response를 사용해서 내보내고 있다. 그런데 sdr 은 entity 를 내보내고 있다.
    * ORM에 response DTO가 어울리는가.....부터 생각
* HATEOS를 글로만 배워봤다. 프론트와 백엔드가 제대로 이해하고 쓸수 있을까?
    * 제대로 사용하지 못하더라도, 조금씩 알아가며 사용하는건 어떨까?
* 일단 적용은 조회 에만 할까?
    * post를 제외한 method는 문제가 없어보이므로, post만 해결하고 사용하는 방안을 찾자 
* application/hal+json에 대해서 제대로 이해하고 가자.
* front 와 backend의 문서 공유방법은? 기존의 swagger의 방법과는 다를것으로 예상됨
* api의 rule가 생기면서 불필요하게 노출되는 것은 아닌지, 보안측면은 어떻게 할지 생각해봐야한다.

##### 논의 결과
1. ORM에 맞는 설계로 가야한다.
2. REST를 제대로 사용하기 위해 해보자.
3. 가장 큰문제는 api 버전이 변경됨에 따라어떻게 처리가 가능한가?!(이문제만 해결되면 해보자.)


##### api versioning 방법
기존의 객체를 사용하지 못하고
새로운 객체가 필요한 경우 api는 동일하나 version이 달라질것으로 예상된다.

1. url versioning을 사용하여
```java
@RepositoryRestResource(path = "v1/stores")

@RepositoryRestResource(path = "v2/stores")
```
2. controller 에서
```java
@RequestMapping(method = RequestMethod.GET, value = "/stores", headers = "x-vendys-version=v4"

```
구 api 버전만을 재정의 한다.  물론 repo부터 재정의 될것이다.


그러면 기존의 경우
새로운 api가 생길때마다 controller -> repo까지 다시만든다.(api가 2개인 경우, 소스가 2벌)  

새로운 방법의 경우  
구 api에 대한 controller -> repo까지 정의가 다시 생기고  
새로운 api는 객체만 정의하게 된다.   

소스코드상의 효율성은 있어보이나....개발자가 헷갈릴수도 있을꺼 같다.



### 기존 inceptor에변경 추가해야 함.
 기존 HandlerInterceptor는 핸들러 매핑의 프로퍼티로 등록된다. 
 #### 두 가지 단점
 1. 핸들러 매핑 전략을 두 개 이상을 병행해서 사용하면 인터셉터를 일일히 핸들러 매핑마다 등록해줘야 한다
 2. 핸들러 매핑에 등록한 인터셉터는 그 핸들러 매핑이 처리하는 모든 요청에 적용된다는 점이다. URL에 따라 적용 인터셉터를 선별하려면 인터셉터 안에서 URL을 필터링 하는 작업을 해야 한다.

MappedInterceptor는 이 두가지 단점을 보완해주는 새로운 인터셉터 적용방식이다. MappedInterceptor를 빈으로 등록하고 path와 interceptor를 지정해주면 해당 경로패턴에 일치하는 URL에 대해서는 어떤 핸들러 매핑을 이용하든 상관없이 지정한 인터셉터를 적용해준다. 일종의 글로벌 스마트 인터셉터 레지스트리라고나 할까. 이 MappedInterceptor의 등장으로 앞으로는 핸들러 매핑에 인터셉터를 등록해서 사용할 일은 없을 듯 하다.

```java 
    @Bean
    public MappedInterceptor dbEditorTenantInterceptor() {
        return new MappedInterceptor(new String[]{"/**"}, dbEditorTenantInterceptor);
    }
```


참조: [좌우충돌 ORM 개발기 중](https://www.slideshare.net/daumdna/devon-2012-b4-orm )
