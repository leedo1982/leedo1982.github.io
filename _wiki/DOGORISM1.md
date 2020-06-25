---
layout  : wiki
title   : REST API 최신버전을 관리하자
summary : how to get lastest Rest API?
date    : 2018-04-20 13:11:22 +0900
updated : 2019-02-09 11:21:01 +0900
tags    : dogorism
toc     : true
public  : true
parent  : Aogorism
latex   : false
---
* TOC
{:toc}

# 논의
rest api의 버전을 제품의 버전과 통일시키면서
어떻게 하면 rest api 버전을 효율적으로 관리할 수 있을지에 관한 논의가 있었다.

시멘틱 버전을 따르면 메이져 버전이 변경 될때 마다 
버전간 하위 호환성을 유지하며, 버전의 변경이 없는 경우에는 최신 버전의 rest-Api 를 찾아주는 알고리즘을 고민하게 되었다.


## logic(version을 header에 관리하는 경우)
>1. 최초의 api는 header에 특정한 버전을 선정하지 않는다.
>2. api의 버전이 올라가는 경우
>    * 기존의 api에 현재 사용중인 version을 header에 확정시킨다.
>    * 새로운 api에는 header을 특정하지 않는다.


## 예시

1. 상황 : 최초의 api 생성  
   현재 최신 버전 : version = 1  
   조치 : 새로운 api에는 header을 명명하지 않는다.  
   
 ```java
 @GetMapping(value = "/student")
  public void creatStudent() {
  }
  
  @PostMapping(value = "/student")
  public void updateStudent() {
  }

  @DeleteMapping(value = "/student")
  public void updateStudent() {
  }
```

2. 상황 : post, student api 버전의 변경이 발생하였다.  
   현재 최신 버전 : version = 2  
   조치 : 구 api에 버전을 header에 명명해주며, 신 api에는 headerd을 명명하지 않는다.  
   
 ```java
 @GetMapping(value = "/student")
  public void creatStudent() {
  }
  
  @PostMapping(value = "/student", header = "version=1")
  public void updateStudent() {
  }
  
  @PostMapping(value = "/student")
  public void updateStudent() {
  }

  @DeleteMapping(value = "/student")
  public void updateStudent() {
  }
```

3. 상황 : delete, student api 버전의 변경이 발생하였다.  
   현재 최신 버전 : version = 3  
   조치 : 구 api에 버전을 header에 명명해주며, 신 api에는 headerd을 명명하지 않는다.  
   
 ```java
 @GetMapping(value = "/student")
  public void creatStudent() {
  }
  
  @PostMapping(value = "/student", header = "version=1")
  public void updateStudent() {
  }
  
  @PostMapping(value = "/student")
  public void updateStudent() {
  }
  
  @DeleteMapping(value = "/student", header = {"version=1","version=2"})
  public void updateStudent() {
  }

  @DeleteMapping(value = "/student")
  public void updateStudent() {
  }
```

4. 위와 같이 버전이 변경될때 마다 반복한다. 


## 문제가 될수도 있을수도 있는점(...)
현재 버전이 3.0 인 경우 그 이상의 버전도 받아들인다는 문제점이 있으나  
실직적으로 그 이상의 버전을 받아들이면서 발생할 문제점이 없을 것으로 예상되어  
이 문제는 논리적으로만 인지하고 넘어간다. 


| 이를 Dogorism no.1으로 명명한다.



