---
layout  : wiki
title   : Mysql 연산자와 내장 함수
summary : 
date    : 2021-02-04 07:35:31 +0900
updated : 2021-02-04 07:49:17 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 개요 
  이번 절에서는 MySql 에서만 사용 가능한 연산자도 함계 살펴보겠지만 가능하다면 SQL의 가독성을 높이기 위해 ANSI 표준형태의 연산자를 사용하길 권장한다.

# 7.3.1 리터럴 표기법 
  - 문자열 
    SQL 표준에서 문자열은 항상 홑따옴표를 사용해서 표시한다. 문자열값에 홑따옴표가 포함돼 있을 때 홑따옴표를 두번 연숙해서 입력하면 된다.
    Mysql 에서 sql_mode 시스템 변수값에 "ANSI_QUOTES"를 설정하면 쌍따옴표는 문자열 리터럴 표기에 사용할 수 없다. 
  - 숫자 
    따옴표 없이 숫자 값을 입력하면 된다. mysql 은 숫자 타입과 문자열 타입 간의 비교에서 숫자 타입을 우선시하므로 문자열 값을 숫자 값으로 변환한 후 비교를 수행한다. 
    주어진 상수값이 숫자 값인데 비교되는 칼럼이 문자열이면 인덱스를 이용하지 못한다. 
  - 날짜 
    mysql 에서는 정해진 형태의 날짜 포맷으로 표기하면 mysql 서버가 자동으로 값을 변환하기 때문에 str_to_date() 같은 함수를 사용하지 않아도 된다.
  - 불리언 
    BOOL이나 BOOLEAN 이라는 타입이 있지만 사실 이것은 TINYINT 타입에 대한 동의어일 뿐이다. 