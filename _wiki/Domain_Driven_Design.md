---
layout  : wiki
title   : 도메인 주도 설계
summary : DDD
date    : 2018-05-12 13:03:31 +0900
updated : 2019-02-09 11:12:33 +0900
tags    : DDD
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 기본요소 네비게이션 맵

##  Model-Driven-Design
* --> 모델을 표현하는데 활용 : [[SERVICE]] , [[ENTITY]] , [[VALUE_OBJECT]] , [[MODULE]]
* --> 모델을 격리하는데 활용 : [[LAYERED_ARCHITECTURE]]
* --> 상호 배타적인 선택 : [[SMART_UI]]

### ENTITY 
* --> 접근하는데 활용 : REPOSITORY
* --> 무결성을 유지, 루트 역할하는데 활용: AGGREGATE
* --> 캡슐화 하는데 활용 : FACTORY

### VALUE_OBJECT
* --> 캡슐화 하는데 활용 : AGGREGATE , FACTORY

#### AGGREGATE
* --> 캡슐화 하는데 활용 : FACTORY

# 상세 
* [[Domain]]
* [[LAYERED_ARCHITECTURE]]
* [[SERVICE]]
* [[ENTITY]]
* [[VALUE_OBJECT]]
* [[MODULE]]
* [[SMART_UI]]
