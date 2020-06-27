---
layout  : wiki
title   : Redis in Spring
summary : 스프링에서 redis 를 사용하여 개발효울성을 높여보자 
date    : 2020-06-26 14:21:24 +0900
updated : 2020-06-26 14:29:16 +0900
tag     : spring redis 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# Redis?
```
Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker.
```
redis 는 오픈소스로 데이터베이스, 캐시, 메세지브로커처럼 사용하는  메모리 내의 데이터 구조 저장소이다.


# Springboot 에서 Redisson 사용
```
<dependency>
   <groupId>org.redisson</groupId>
   <artifactId>redisson</artifactId>
   <version>3.13.1</version>
</dependency>  
```


