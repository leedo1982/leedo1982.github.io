---
layout  : wiki
title   : 실행계획 분석
summary : 
date    : 2020-12-22 22:45:18 +0900
updated : 2020-12-24 07:46:33 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 개요 
  explain 명령만 사용하면 기본적인 쿼리 실행 계획만 보인다. 하지만 explain extended 나 explain partitions 명령을 이용해 더 상세한 실행 계획을 확인할 수 있다. 
  explain 을 실행하면 쿼리 문장의 특성에 따라 표 형태로 된 1줄 이상의 결과가 표시된다. 표의 각 라인은 쿼리 문장에서 사용된 테이블의 개수만큼 출력된다. 실행순서는 위에서 아래로 순서대로 표시된다. 출력된 실행 계획에서 위쪽에 출력된 결과일수록 쿼리의 바깥 부분이거나 먼저 접근한 테이블이고, 아래쪽에 출력된 결과 일수록 쿼리의 안쪽 부분 또는 나중에 접근한 테이블에 해당한다. 
  
## 6.2.1 id 칼럼 
  실행계획에서 가장 왼쪽에 표시되는 id 칼럼은 단위 SELECT 쿼리별로 부여되는 식별자 값이다. 만약 하나의 SELECT 문장안에서 여러개의 테이블을 조인하면 조인되는 테이블의 개수만큼 실행 계획 레코드가 출력되지만 같은 id가 부여된다.
  반대로 다음 쿼리의 실행 계획에서는 쿼리문장이 3개의 단위 SELECT 쿼리로 구서오대 있으므로 실행계획의 각 레코드가 각기 다른 id 를 지닌 것을 확인할 수 있다.
  ```
  EXPLAIN
  SELECT
  ((SELECT COUNT(*) FROM employees) + (SELECT COUNT(*) FROM departments)) AS total_count;
  ```
  
## 6.2.2 select_type 칼럼  
  각 단위 SELECT 쿼리가 어떤 타입의 쿼리인지 표시되는 칼럼이다. 
  - SIMPLE : UNION 이나 서브 쿼리를 사용하지 않는 단순한 SELECT 쿼리인 경우. 쿼리 문장이 아무리 복잡하더라도 실행계획에서 select_type 이 SIMPLE 인 단위쿼리는 반드시 하나만 존재한다.
  - PRIMARY : UNION 이나 서브쿼리가 포함된 SELECT 쿼리의 실행계획에서 가장 바깥쪽에 있는 단위 쿼리. SIMPLE 와 마찬가지로 PRIMARY 인 단위 SELECT 는 하나만 존재한다.
  - UNION : UNION 으로 결합하는 단위 SELECT 쿼리 가운데 첫번째를 제외한 두번째 이후 단위 SELECT 쿼리. 첫번째 단위 SELECT 는 UNION 이 아니라 UNION 쿼리로 결합된 전체 집합의 select_type 이 표시된다.
  - DEPENDENT UNION : UNION 과 같이 쿼리에 UNION 이나 UNION ALL 로 집합을 결합하는 쿼리에 표시. DEPENDENT 는 UNION 이나 UNION ALL 로 결합된 단위 쿼리가 외부의 영향에 의해 영향을 받는 것을 의미한다.
  - UNION RESULT : UNION 결과를 담아두는 테이블을 의미한다.  Mysql 에서 UNION ALL 이나 UNION 쿼리는 모두 UNION 의 결과를 임시 테이블로 생성하게 되는데, 실행계획상에서 이 임시 테이블을 가리키는 라인의 select_type 이 UNION RESULT 이다. 실제 단위쿼리가 아니기 때문에 id 는 부여되지 않는다.
  - SUBQUERY : FROM 절 이외에서 사용되는 서브 쿼리만을 의미한다. Mysql 의 실행계획에서 FROM 절에 사용된 서브쿼리는 DERIVED 라고 표시되고, 그밖의 위치에서 사용된 서브쿼리는 전부 SUBQUERY 라고 표시된다.
  - DEPENDENT SUBQUERY : 서브쿼리가 바깥쪽 SELECT 쿼리에서 정의된 칼럼을 사용하는 경우 
  ```
  EXPLAIN
  SELEXT e.first_name,
    (SELECT COUNT(*)
    FROM dept_emp de, dept_manager dm
    WHERE dm.dept_no=de.dept_no AND de.emp_no=e.emp_no) AS cnt
  FROM employees e
  WHERE e.emp_no=10001; // dm, de = DEPENDENT SUBQUERY 
  ```
  - DERIVED : 서브쿼리가 FROM 절에 사용된 경우 MySql은 항상 select_type이 DERIVED 인 실행계획을 만든다. DERIVED 인 경우에 생성되는 임시테이블을 파생테이블이라고도 한다. 안타깝게도 Mysql 은 FROM 절에 사용된 서브쿼리를 제대로 최적화하지 못할때가 대부분이다. 파생테이블에는 인덱스가 전혀 없으므로 다른 테이블과 조인할 때 성능상 불리할 때가 많다. 
  ```
  EXPLAIN
  SELECT *
  FROM (SELECT de.emp_no, FROM dept_emp de) tb, employees e
  WHERE e.emp_no = tb.emp_no; // de = DERIVED 
  ```
  - UNCACHEABLE SUBQUERY : 서브쿼리에 포함된 요소에 의해 캐시 자체가 불가능할수 있을때 표시된다. 하나의 쿼리 문장에서 서브쿼리가 하나만 있더라도 실제 그 서브 쿼리가 한번만 실행되는 것은 아니다. 그런데 조건이 똑같은 서브쿼리가 실행될 때는 다시 실행하지 않고 이전의 실행결과를 그대로 사용할 수 있게 서브쿼리의 결과를 내부적인 캐시 공간에 담아둔다. 서브쿼리 캐시는 쿼리캐시나 파생테이블과는 전혀 무관하다. 
    - 사용자 변수가 서브쿼리에 사용된경우
    - NOT-DETERMINISTIC 속성의 스토어드 루틴이 서브쿼리 내에 사용된 경우
    - UUID()나 RAND() 와 같이 결과값이 호출할 때마다 달라지는 함수가 서브쿼리에 사용된 경우 
  - UNCACHEABLE UNION : 두개의 키워드의 속성이 혼합된 select_type 을 의미한다. 


## 6.2.3 table 칼럼 
  Mysql 의 실행계획은 단위 SELECT 쿼리 기준이 아니라 테이블 기준으로 표시된다. 별칭이 부여된경우 별칭이 표시된다. 별도의 테이블을 사용하지 않는 경우 NULL 이 표시된다. table 칼럼에는 "<>" 로 이름이 명시되는 경우가 많은데, 이 테이블은 임시테이블을 의미한다. 안의 표시되는 숫자는 SELECT 쿼리의 id 를 지칭한다. 

    
