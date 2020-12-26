---
layout  : wiki
title   : 실행계획 분석
summary : 
date    : 2020-12-22 22:45:18 +0900
updated : 2020-12-26 18:51:46 +0900
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

    
## 6.2.4 type 칼럼 
 각 테이블의 레코드를 어떤 방식으로 읽었는지를 의미한다. 여기서 방식이라함은 인덱스를 사용해 레코드를 읽었는지 아니면 테이블을 처음부터 끝까지 읽는 풀 테이블 스캔으로 읽었는지 등을 의미한다.  
 mysql 에서는 type를 조인 타입으로 소개한다. 하지만 크게 조인과 연관지어 생각하지 말고, 각 테이블의 접근방식으로 해석하면 된다.  
 ``` 
 성능이 빠른 순서
 system - const - eq_ref - ref - fulltext - ref_or_null 
 - unique_subquery - index_subquery - range - index_merge - index 
 - ALL
 (ALL : 풀테이블 스캔)
 ```
 - system : 레코드가 1건만 존재하는 테이블 또는 한건도존재하지 않는 테이블을 참조하는 형태의 접근 방법
 - const : 테이블의 레코드의 건수에 관계없이 쿼리가 프라이머리 키나 유니크 키 칼럼을 이용하는 WHERE 조건절을 가지고 있으며, 반드시 1건을 반환하는 쿼리의 처리방식 , 다중칼럼으로 구성된 프라이머리 키나 유니크 키중에서 인덱스의 일부 칼럼만 조건으로 사용할 때는 const 타입을 사용할 수 없다. 
 ```
 EXPLAIN
 SELECT * FROM employees WHERE emp_no = 10001;
 ```
 - eq_ref : 여러 데이블이 조인되는 쿼리의 실행 계획에서만 표시된다. 조인에서 처음 읽은 테이블의 칼럼 값을, 그 다음 읽어야할 테이블의 프라이머리 키나 유니크 키 칼럼의 검색 조건에 사용할 때를 eq_ref 라고 한다. 
 ```
 EXPLAIN
 SELECT * FROM dept_emp de, employees e
 WHERE e.emp_no = de.emp_no AND de.dept_np='d005';
 ```
 - ref : eq_ref 와 달리 조인의 순서와 관계없이 사용되면, 프라이머리키나 유니크 키등의 제약 조건도 없다. 인덱스의 종류와 관계없이 동등 조건으로 검색할 때는 ref 접근방법이 사용된다. 
 ```
 EXPLAIN 
 SELECT * FROM dept_emp WHERE dept_no = 'd005';
 ```
---
이 세가지 접근방식은 보두 WHERE 조건절에서 사용되는 비교연산자는 동등 비교연산자이어야 한다. 매우 좋은 접근 방법으로 인덱스의 분포도가 나쁘지 않다면 성승상의 문제를 일으키지 않는 접근 방법이다.  

 - fulltext : 전문 검색 인덱스를 사용해 레코드를 읽는 접근 방법을 의미한다. 전문검색은 "MATCH ... AGAINST ... " 구문을 사용하는데, 반드시 해당 테이블에 전문 검색용 인덱스가 준비돼 있어야만 한다. 
 ```
 EXPLAIN
 SELECT *
 FROM employee_name
 WHERE emp_no = 10001
 AND emp_no BETWEEN 10001 AND 10005
 AND MATCH(first_name, last_name) AGAINST ('Facello' IN BOOLEAN MODE);
 ```
 - ref_or_null : ref 접근 방식과 같은데, NULL 비교가 추가된 형태이다. 실제 업무에서 많이 보이지도 않고, 별로 존재감이 없으므로 대략의 의미만 기억해두어도 충분하다.
 - unique_subquery : where 조건절에서 사용될 수 있는 in (subquery) 형태의 쿼리를 위한 접근 방식이다. 의미 그대로 서브쿼리에서 중복되지 않은 유니크한 값만 반환할 때 이 접근 방법을 사용한다. 
 ```
 EXPLAIN
SELECT * FROM departments WHERE dept_no IN (
    SELECT dept_no FROM dept_emp WHERE emp_no = 10001
        );
 ```
 - index_subquery : IN 연산자의 특성상 , IN (subquery) 또는 IN (상수 나열) 형태의 조건은 괄효 안에 있는 값의 목록에서 중복된 값이 먼저 제거돼야 한다. subquery 가 중복된 값을 반환할 수는 있지만 중복된 값을 인덱스를 이용해 제거할 수 있을때 index_subquery 접근 방법이 사용된다.
 ```
 EXPLAIN
SELECT * FROM departments WHERE dept_no IN (
    SELECT dept_no FROM dept_emp WHERE dept_no BETWEEN 'd001' AND 'd003'
        );
 ```
 - range : 익히 알고 있는 인덱스 레인지 스캔 형태의 접근 방법이다. 주로 "<,>, IS NULL, BETWEEN, IN, LIKE"등의 연산자를 이용해 인덱스를 검색할 때 사용된다. 
 ```
 EXPLAIN
 SELECT dept_no FROM dept_emp WHERE dept_no BETWEEN 'd001' AND 'd003'
 ```
 - index_merge : 2개 이상의 인덱스를 이용해 각각의 검색 결과를 만들어낸 후 그 결과를 병합하는 처리 방식이다. 하지만 여러번의 경험을 보면 이름만큼 효율적으로 작동하는 것 같지는 않았다. 
 ```
 EXPLAIN
 SELECT * FROM employees
 WHERE emp_no BETWEEN 10001 AND 11000
    OR first_name = 'Smith';
 ```
 - index : 인덱스를 처음부터 끝까지 읽는 인덱스 풀 스캔을 의미한다. range 방식 과 같이 효울적으로 인덱스의 필요한 부분만 읽는 것을 의미하는 것은 아니라는 점을 잊지 말자. 
 - ALL : 풀테이블 스캔을 의미하는 접근 방식이다. 
   

## 6.2.5 possible_keys 
 Mysql 옵티마이져가 최적의 실행계획을 만들기 위해 후보로 선정했던 접근 방식에서 사용되는 인덱스의 목록일 뿐이다. 즉, 말 그대로 "사용될 법했던 인덱스의 목록"인 것이다. 
 
## 6.2.6 key 
  최종 선택된 실행 계획에서 사용되는 인덱스를 의미한다. 
  
## 6.2.7 key_len 
  실행계회의 key_len 칼럼의 값은, 쿼리를 처리하기위해 다중 컬럼으로 구성된 인덱스에서 몇개의 칼럼이 사용되었는지 우리에게 알려준다. 더 정확하게는 인덱스의 각 레코드에서 몇 바이트까지 사용했는지 알려주는 값이다.
  
## 6.2.8 ref 
  접근 방법이 ref 방식이면 참조 조건으로 어떤 값이 제공됐는지 보여준다. 만약 상수 값을 지정했다면 ref 칼럼의 값은 const 로 표시되고, 다른테이블의 칼럼값이면 그 테이블의 명과 칼럼이 표시된다. 
## 6.2.9 rows 
  실행계획의 효율성 판단을 위해 예측했던 레코드 건수를 보여준다. 예상값으로 정확하지는 않다. 또한 row 칼럼에 표시되는 값은 반환되는 레코드의 예측치가 아니라 쿼리를 처리하기 위해 얼마나 많은 레코드를 디스크로부터 읽고 체크해야 하는지를 의미한다.   
  
## 6.2.10 Extra  
  쿼리의 실행 계획에서 성능에 관한 중요한 내용이 표시된다 
  - const row not found : const 접근 방식으로 테이블을 읽었지만 실제로 해당 테이블에 레코드가 1건도 존재하지 않는 경우
  - Distinct : 쿼리의 Distinct 를 처리하기 위해 조인하지 않아도 되는 항목은 모두 무시하고 꼭 필요한 것만 조인했으며, 꼭필요한 레코드만 읽었다는 것을 표현하고 있다
  - Full scan on NULL key : "col1 IN (SELECT col2 FROM ...)" 과 조건이 포함된 가진 쿼리에서 자주 발생할 수 있는데, col1 이 NULL 을 만나면 예비책으로 풀 테이블 스캔을 사용할 것이라는 사실을 알려주는 키워드이다.
  - Impossible HAVING : 쿼리에 사용된 HAVING 절의 조건을 만족하는 레코드가 없을 때 표시된다.
  - Impossible WHERE : WHERE 조건이 항상 FALSE 가 될 수 밖에 없는 경우
  - Impossible WHERE noticed after reading const tables : 실제 데이터를 읽어보지 않고도 바로 테이블 구조상으로 불가능한 조건을 넘어 쿼리의 일부분을 실행해본 결과시 표시
  - No matching min/max row : 만약 MIN() 이나 MAX() 같은 집합 함수가 있는 쿼리의 조건절에 일치하는 레코드가 한건도 없을 때 표시된다.
  - No matching row in const table : 사용된 테이블에서 const 방식으로 접근할 때, 일치하는 레코드가 없다면 표시된다.
  - No tables used : FROM 절이 없는 쿼리문장이나 "FROM DUAL" 형태의 쿼리 실행 계획에서 표시된다.
  - Not exists : 프로그램을 개발하다 보면 A 테이블에는 존재하지만 B 테이블에는 없는 값을 조회해야 하는 형태의 안티-조인 방식을 사용하는데 이럴때 표시된다.
  - Range checked for each record(index map: N) : 매 레코드마다 인덱스 레이지 스캔을 체크할 때 표시된다.
  - Scanned N database : 서버 내에 존재하는 DB의 메타정보를 INFORMATION_SCHEMA DB 에 모아둔다. 실제로 이 dB 내의 테이블은 레코드가 있는것이 아니라, SQL 을 이용해 조회할때마다 메타 정보를 MySQL 서버의 메모리에서 가져와서 보여준다. 이런 이유로 한꺼번에 많은 테이블을 조회할 경우 시간이 많이 걸리고, 몇개의 정보를 읽었는지 N 을 통해 보여둔다.
    - 0 : 특정 테이블의 정보만 요청되어 데이터베이스 전체의 메타 정보를 읽지 않음
    - 1 : 특정 데이터베이스내의 모든 스키마 정보가 요청되어 해당 데이터베이스의 모든 스키마 정보를 읽음
    - ALL : Mysql 서버 내의 모든 스키마 정보를 다 읽음
  - Select tables optimized away : MIN() 또는 MAX() 만 SELECT 절에 사용되거나 또는 GROUP BY 로 MIN(),MAX() 를 조회하는 쿼리가 적절한 인덱스를 사용할 수 없을 때 인덱스를 오름차순 또는 내림차순으로 1건만 읽는 형태의 최적화가 적용된다면 표시된다.
  - INFORMATION_SCHEMA DB를 SELECT 할때
    - Skip_open_table : 테이블의 메타 정보가 저장된 파일을 별도로 읽을 필요가 없음
    - Open_frm_only : 테이블의 메타 정보가 저장된 파일(*.FRM)만 열어서 읽음
    - Open_trigger_only : 트리거 정보가 저장된 파일(*.TRG) 만 열어서 읽음
    - Open_full_table : 최적화되지 못해서 테이블 메타 정보파일과 데이터 및 인덱스 파일까지 모두 읽음
  - unique row not found : 두 개의 테이블이 각각 유니크(PK 포함) 칼럼으로 아우터 조인을 수행하는 쿼리에서 아우터 테이블에 일치하는 레코드가 존재하지 않을 때 표시된다.
  - Using filtsort : ORDER BY 를 처리하기 위해 인덱스를 이용할 수도 있지만 적절한 인덱스를 사용하지 못하여 다시 정렬할 때 표시된다.
  - Using index(커버링 인덱스) : 데이터 파일을 전혀 읽지 않고 인덱스만 읽어서 쿼리를 모두 처리할 수 있을 때 표시된다.
  - Using index for group-by : GRUOP BY 처리가 인덱스를 이용하면 정렬된 인덱스 칼럼을 순서대로 읽으며서 그룹핑 작업만 숭핸다. 이렇게 처리하면 레코드의 정렬이 필요하지 않고 인덱스의 필요한 부분만 읽으면 되기 때문에 상당히 효울적이고 빠르게 처리되며 그때 표시된다.
  - Using join buffer : 드리븐 테이블 검색을 위한 적절한 인덱스가 없다면 드라이빙 테이블로 부터 읽은 레코드이 건수만큼 매번 드리븐 테이블을 풀 테이블 스캔이나 인덱스 풀 스캔을 해야한다. 이때 비효을을 보완하기 위헤 Mysql 서버는 드라이빙 테이블에서 읽은 레코드를 임시공간에 보관해두고 필요할 때 재사용하게 해준다. 읽은 레코드를 임시로 보관해두는 메모리 공간을 "조인버퍼" 라고하며, 이때 표시된다.
  - 쿼리가 Index_merge 접근방식으로 실행되는 경우 2개 이상의 인덱스가 동시에 사용될 수 있다. 이때 아래 3개 중하나의 메세지를 출력한다.
    - Using intersect(...) : 각각의 인덱스를 사용할 수 있는 조건이 AND 로 연결된 경우 각 처리결과에서 교집합을 추출해내는 작업을 수행했다는 의미다.
    - Using union(...) : 각 인덱스를 사용할 수 있는 조건이 OR 로 연결된 경우 각 처리결과에서 합집합을 추출해내는 작업을 수행했다는 의미다.
    - Using sort_union(...) : Using union 과 같은 작업을 수행하지만 Unsing union 으로 처리될 수 없는 경우 이방식으로 처리된다.
  - Using trmporary : 쿼리를 처리하는 동안 중간 결과를 담아두기 위해 임시 테이블을 사용한다. 임시테이블은 메모리상에 생성될 수도 있고 디스크상에 생성될 수도 있다.
  - Using where : Mysql 엔진 레이어에서 별도의 가공을 해서 필터링과 작업을 처리한 경우에만 표시한다.
  - Using where with pushed condition : "condition push down"이 적용됐음을 의미하는 메시지다. 이메시지는 NDB 클러스터 스토리지 엔진을 사용하는 테이블에서만 표시되는 메시지다.

## 6.2.11 EXPLAIN EXTENDED(Filtered 칼럼)
Mysql 5.1.12 버전부터는 필터링이 얼나마 효율적으로 실행됐는지 사용자에게 알려주기 위해 실행계획에 Filtered 라는 칼럼이 새로 추가됐다. EXPLAIN EXTENDED 를 사용하면 확인할 수 있다. Mysql 엔진에 의해 필터링되어 제거된 레코드는 제외하고 최종족으로 레코드가 얼마나 남았는지 비율이 표시된다.

## 6.2.12 EXPLAIN EXTENDED(추가 옵티마이저 정보)
EXPLAIN EXTENDED 명령을 실행해 실행계획이 출력된 직후 "SHOW WARNING" 명령을 싱행하면 옵티마이저가 분석해서 다시 재조합한 쿼리 문장을 확인할 수 있다. 옵티마이저가 쿼리를 어떻게 해석했고, 어떻게 쿼리를 변환했으며, 어떤 특수한 처리가 수행됐는지등을 판단힐 수 있으므로 알아두면 도움될 것이다. 

## 6.2.13 EXPLAIN PARTITIONS(partitions 칼럼)
쿼리를 실행하기 위헤 테이블의 파티션 중에서 어떤 파티션을 사용했는지 등의 정보를 조회할 수 있다. 


      
      
