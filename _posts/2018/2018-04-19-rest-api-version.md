---
layout  : post
title   : rest api version
summary : restful
date    : 2018-04-19 09:48:40 +0900
updated : 2018-04-19 10:10:55 +0900
tags    : rest
toc     : true
public  : true
---
* TOC
{:toc}

# 논의의 출발
REST API를 사용하며 새로운 버전으로 통합하고 싶다는 needs가 발생했다.
그래서 REST api 에 version 관리를 어떠한 방식으로 할것인지 회의를 한다.

논점:
1. api의 버전을 어떻게 표현할 것인가 ?



## rest api 버전 표현 방법
1. URL Versioning
- http://localhost:8080/v1/person
- http://localhost:8080/v2/person

```java

@RestController
public class StudentVersioningController {

  @GetMapping("v1/student")
  public StudentV1 studentV1() {
    return new StudentV1("Bob Charlie");
  }

  @GetMapping("v2/student")
  public StudentV2 studentV2() {
    return new StudentV2(new Name("Bob", "Charlie"));
  }

```

2. Request Parameter Versioning
- http://localhost:8080/person/param?version=1
- http://localhost:8080/person/param?version=2

```java
  @GetMapping(value = "/student/param", params = "version=1")
  public StudentV1 paramV1() {
    return new StudentV1("Bob Charlie");
  }

  @GetMapping(value = "/student/param", params = "version=2")
  public StudentV2 paramV2() {
    return new StudentV2(new Name("Bob", "Charlie"));
  }
```

3. (Custom) Headers Versioning
- http://localhost:8080/person/header  (headers=[X-API-VERSION=1])
- http://localhost:8080/person/header    (headers=[X-API-VERSION=2])

```java
  @GetMapping(value = "/student/header", headers = "X-API-VERSION=1")
  public StudentV1 headerV1() {
    return new StudentV1("Bob Charlie");
  }

  @GetMapping(value = "/student/header", headers = "X-API-VERSION=2")
  public StudentV2 headerV2() {
    return new StudentV2(new Name("Bob", "Charlie"));
  }
```

4. Media type Versioning(aka 'content negotiation' or 'accept header')
- http://localhost:8080/person/produces  (headers[Accept=application/vnd.company.app-v1+json])
- http://localhost:8080/person/produces  (headers[Accept=application/vnd.company.app-v2+json])

```java
  @GetMapping(value = "/student/produces", produces = "application/vnd.company.app-v1+json")
  public StudentV1 producesV1() {
    return new StudentV1("Bob Charlie");
  }

  @GetMapping(value = "/student/produces", produces = "application/vnd.company.app-v2+json")
  public StudentV2 producesV2() {
    return new StudentV2(new Name("Bob", "Charlie"));
  }
```

## 누가 사용하고 있나?
* Media type versioning : GitHub
* (Custom) Headers versioning : Microsoft
* URI Versioning : Twitter
* Request Parameter versioning : Amazon

## 나의 취향은?(개인의 취향을 존중하며)
**개인적인 의견으로 header 방식을 선호한다.**  
url 방식은 사용자에게 노출될 수있는 부분이기에 꺼리며,  
media type 방식은 restful 한 방식이긴 할꺼 같지만 등록 및 관리의 번거로움이 존재할 것 같음.  
param 방식은 일부러 get 방식이 아닌 method 에서 까지 param으로 버전을 관리하는 것은 불필요해보이기 때문이다.  

물론 header 방식을 사용하면 살짝 숨겨져 있어 프론트와의 소통시 주의를 더 갖어야 할것이다.







**출처:**
[Versioning RESTful Services](http://www.springboottutorial.com/spring-boot-versioning-for-rest-services )


