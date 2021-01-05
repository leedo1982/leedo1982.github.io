---
layout  : wiki
title   : 매뉴얼의 SQL 문법 표기를 읽는 방법 
summary : 
date    : 2021-01-06 07:55:13 +0900
updated : 2021-01-06 08:09:26 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 개요 
  Mysql 매뉴얼에 명시된 SQL 문법은 사용할 수 있는 모든 키워드나 기능을 하나의 문장에 다표기해뒀기 때무ㅜㄴ에 한눈에 이해되지 않는다는 단점이 있다. 
  ```
  INSERT [LOW_PRIORITY | DELAYED | HIGH{PRIORITY ] [IGNORE]  
    [INTO] tbl_name
    SET col_name  = {expr | DEFAULT} ...
    [ON DUPLICATE  KEY UPDATE
        col_name = expr
        [, col_name = expr]....]
  ```
  표현식이 표기된 순서대로만 사용할 수 있다. 위 표기법에서 대문자로 표기된 단어는 모두 키워드를 의미한다.  
  이탤릭체로 표현된 단어는 사용자가 선택해서 작성하는 토큰을 의미하는데 대부분 테이블명이나 컬럼명 또는 표현식을 사용한다.   
  대괄호는 해당 키워드나 표현식 자체가 선택사항임을 의미힌다.  
  파이프는앞과 뒤의 키워드나 표현식중에서 단 하나만 선택해서 사용할 수 있음을 의미한다.  
  중괄호는 괄호내 아이템 중에서 반드시 하나를 사용해야 하는 경우를 의미한다.  
  "..." 표기는 앞에 명시된 키워드나 표현식의 조합이 반봅될 수 있음을 의미한다.
 
