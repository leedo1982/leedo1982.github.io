---
layout  : wiki
title   : Hypertext Application Language
summary : HAL
date    : 2018-05-15 13:23:40 +0900
updated : 2018-05-16 09:29:48 +0900
tags    : HAL
toc     : true
public  : true
parent  : Spring
latex   : false
---
* TOC
{:toc}

# 개요
HAL은 API의 리소스 간을 하이퍼링크하는 일관되고 쉬운 방법을 제공하는 간단한 형식이다.
HAL을 채택하면 API를 탐색할 수 잇으며 API 자체에서 문서를 쉽게 찾을 수 있다.
간단히 말해, API를 사용하기가 더 쉬워지고 클라이언트 개발자가 더 매력적으로 보일것이다.


## 규칙
HAL 규칙은 리소스(Resource)와 링크(Links)를 표현한다.

### 자원(RESOURCE)
* URI에 대한 링크
* 임베디드 리소스
* 상태(늪지 표준 JSON or XML 데이터)

### 링크(LINKS)
* 타겟(URI)
* 관계 일명. 'rel'(링크이름)
* 지원 중단, 콘텐츠 협상 등을 돕는 몇가지 다른 선택적 속성


## HAL 이 API에서 사용되는 방법
HAP은 클라이언트가 링크를 따라 리소스를 탐색하는 API를 작성하기 위해 설계되었습니다.
링크는 링크관계로 식별됩니다. 링크관계는 하이퍼미디어 API의 핵심요소입니다.
링크관계는 HAL의 식별문자열이 아닙니다. 그것들은 실제로 URL이며, 개발자는 주어진 링크에 대한 문서를 읽을 수 있습니다.

### HAL은 다음에 대한 링크관계 사용을 권장한다.
* 표현 내에서 링크 및 임베디드 리소스를 식별합니다.
* 대상 자원의 예상되는 구조와 의미를 추측합니다.
* 대상 자원에 요청 및 표현을 제출할 수 있음을 알리는 신호

## HAL 문서의 구조
### Minimum valid document(최소 유효 문서)
HAL 문서에는 적어도 비어이는 자원이 있어야 한다.
```json
{}  
```

### Resources(자원)
대부분의 경우, 리소스에는 자체 URI가 있어야 한다.
```json
{
    "_links": {
        "self": { "href": "/example_resource" }
    }
}
```

### Links(링크)
링크는 리소스 내에 직접 포함되어야 한다.
링크는 _links 자원 객체의 직접 특성 인 해시 내에 포함된 JSON객체로 표현된다.

{
    "_links": {
        "next": { "href": "/page=2" }
    }
}

#### Link Relations(링크 관계)
링크에는 관계가 있다.(일명 'rel') 
이것은 특정 링크의 의미를 나타낸다.

링크관계는 자원의 링크를 구별하는 주요 방법이다.
이것은 기본적으로 _links 내의 key 이며 실제 'href' 값과 같은 데이터가 포함된 링크 객체와 링크의미('rel')을 연결한다.
```json 
{
    "_links": {
        "next": { "href": "/page=2" }
    }
}
```

#### API Discoverability(API 검색 가능성)
링크 rels는 해당 링크에 대한 문서를 공개하여 발견 가능하게 만드는 URL 이어야 합니다.


### Representing Multiple Links With The Same Relation(같은 관계로 여러링크 표현)
리소스는 동일한 링크 관계를 공유하는 여러 링크가 있을수 있다. 
```json
{
    "_links": {
      "items": [{
          "href": "/first_item"
      },{
          "href": "/second_item"
      }]
    }
}
```
note: 링크가 단수인지 여부가 확실하지 않은 경우 링크가 여러개인 것으로 가정한다.

### CURIEs
"CURIE"는 리소스 문서에 대한 링크를 제공한다.

```json
"_links": {
  "curies": [
    {
      "name": "doc",
      "href": "http://haltalk.herokuapp.com/docs/{rel}",
      "templated": true
    }
  ],

  "doc:latest-posts": {
    "href": "/posts/latest"
  }
}
```

