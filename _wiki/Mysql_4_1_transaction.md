---
layout  : wiki
title   : 4.1 트랜잭션
summary : 
date    : 2020-10-13 05:47:54 +0900
updated : 2020-10-19 08:04:37 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 개요
논리적인 작업 셋을 모두 완벽하게 처리하거나 또는 처리하지 못할경우에는 원상태로 복구해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어 주는 기능이다

잠금과 트랜잭션은 서로 비슷한 개념 같지만, 잠금은 동시성 제어를 이한 기능이고 트랜잭션은 데이터의 정합성을 보장하기 위한 기능이다.

# 4.1.1 Mysql 에서의 트랜잭션
 
기존 pk 3이 있는상태에서 
```
INSERT INTO table (fdpk) values (1),(2),(3);
```
실행한 경우
MyISAM은 3에서 오류를 발생하지만 1,2 는 Insert 되어 있다,
InnoDB는 3에서 오류를 발생시키고 1,2 도 실패되어 있다.

# 4.1.2 주의사항
트랜잭션 또한 DBMS 의 커넥션과 동일하게 꼭 필요한 최소의 코드에만 적용하는 것이 좋다. 이는 프로그램 코드에서 트랜잭션의 범위를 최소화하라는 의미이디ㅏ.


