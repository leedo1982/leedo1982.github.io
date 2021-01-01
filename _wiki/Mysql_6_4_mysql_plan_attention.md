---
layout  : wiki
title   : 실행 계획 분석시 주의사항 
summary : 
date    : 2021-01-01 10:44:06 +0900
updated : 2021-01-01 11:01:21 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 개요 
  쿼리의 실행계획만으로도 상당히 내용이 많아서 모두 기억하자면 상당히 힘들 것이다. 그래서 여기는 쿼리의 실행계획을 확인할 때 각 칼럼에 표시되는 값 중에서 특별히 주의해서 확인해야 하는 항목만 간략하게 정리했다.
  
# 6.4.1 Select_type 칼럼의 주의 대상 
  - DERIVED  
    DERIVED 는 FROM 절에서 사용된 서브 쿼리로부터 발생한 임시 테이블을 의미한다. 임시테이블은 메모리에 저장될 수도 있고 디스크에 저장될 수도 있다. 일반적으로 메모리에 저장하는 경우에는 크게 성능에 영향을 미치지 않지만, 데이터의 크기가 커서 임시 테이블을 디스크에 저장하면 성능이 떨어진다. 
  - UNCACHEABLE SUBQUERY  
    쿼리의 FROM 절 이외의 부분에서 사용하는 서브쿼리는 가능하면 Mysql 옵티마이저가 최대한 캐시 되어 재사용 될 수 있게 유도한다. 하지만 사용자 변수나 일부 함수가 사용된 경우에는 이러한 캐시 기능을 사용할 수 없게 만든다. 이런 실행계획이 사용된다면 혹시 사용자 변수를 제거하거나 다른 함수로 대체해서 사용 가능할지 검토해보는 것이 좋다.
  - DEPENDENT SUBQUERY  
    쿼리의 FROM 절 이외이 부분에서 사용하는 서브 쿼리가 자체적으로 실행되지 못하고 외부 쿼리에서 값을 전달받아 실행되는 경우 표시된다. 이는 서브 쿼리가 먼저 실행되지 못하고, 서브 쿼리가 외부 쿼리의 결과 값에 의존적이기 때문에 전체 쿼리의 성능을 느리게 만든다. 서브 쿼리가 불필요하게 외부 쿼리의 값을 전달받고 있는지 검토해서, 가능하다면 외부 쿼리와의 의존도를 제거하는 것이 좋다.

# 6.4.2 Type 칼럼의 주의 대상 
  - ALL, index  
    index 는 인덱스 풀 스캔을 의미하며, ALL 은 풀 테이블 스캔을 의미한다. 둘다 대상의 차이는 있지만 느리다. 새로은 인덱스를 추가하거나 쿼리의 요건을 변경해서 이러한 접근 방법을 제거하는 것이 좋다. 
    
# 6.4.3 Key 칼럼의 주의 대상 
  쿼리가 인덱스를 사용하지 못할 때 실행계획의 Key 칼럼이 아무값도 표시되지 않는다. 쿼리가 인덱스를 사용할 수 있게 인덱스를 추가하거나 ,WHERE 조건을 변경하는 것이 좋다.
  
# 6.4.4 Row 칼럼의 주의대상 
  - 쿼리가 실제 가져오는 레코드 수보다 훨씬 큰값이 Rows 칼럼에 표시되는 경우 쿼리가 인덱스를 정상적으로 사용하고 있는지, 그 인덱스가 충분히 작업범위를 좁혀 줄수 있는 칼럼으로 구성됐는지 검토해보는 것이 좋다.
  - Rows 칼럼의 수치를 판단할 때 주의해야 할 점은 LIMIT가 포함된 쿼리라 하다라도 LIMIT의 제한은 Rows 칼럼의 고려대상에서 제외된다는 것이다.

# 6.4.5 Extra 칼럼의 주의 대상 
  쿼리를 튜닝할 때 중요한 단서가 되는 내용이 많이 표시된다. 
  - 쿼리가 요건을 재대로 반영하고 있는 지 확인 해야 하는 경우 
    - full scan on null key
    - impossible having
    - impossible where
    - impossible where noticed after reading const tables
    - no matching min/max row
    - no matching row in const table
    - unique row not found

  - 쿼리의 실행 계획이 좋지 않은 경우 
    - range checkec for each record
    - using filesort
    - using join buffer
    - using temporary
    - using where 
    쿼리를 더 최적화 할 수 있는지 검토해보는 것이 좋다. 
    
  - 쿼리의 실행계획이 좋은경우  
    - distinct
    - using index
    - using index for group-by

