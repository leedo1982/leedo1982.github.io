---
layout  : wiki
title   : 기타 주의사항 
summary : 
date    : 2020-11-04 07:29:05 +0900
updated : 2020-11-04 07:42:17 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 소개 
  - 스토리지 엔진별 지원 인덱스 목록  
    인덱스는 Mysql 엔진 레벨이 아니라 스토리지 엔진 레벨에 포함되는 영역이므로 스토리지 엔진의 종류별로 사용가능한 인덱스의 종류가 다르다.  


    | 스토리지 엔진      | 인덱스 알고리즘 종류                          |
    | --                 | ---                                           |
    | MyISAM             | B-Tree, R-Tree(Spatial-index), Fulltext-index |
    | InnoDB             |                    B-Tree,                    |
    | Memory             |                 B-Tree, Hash                  |
    | TokuDB             |                 Fractal-Tree                  |
    | NDB(Mysql Cluster) |                B-Tree,    Hash                |
  - analyze 와 optimize의 필요성 
    MyISAM 이나 InnoDB 테이블의 경우, 인덱스에 대한 통계 정보를 관리하고 각 통계 정보를 기반으로 쿼리의 실행계획을 수립한다. 인덱스 통계 정보는 아래와 같이 확이할 수 있다.  
    ```
    $> SHOW INDEX FROM tb_test;
    ```
    여기서 가장 중요한 칼럼은 Cardinalty 항목이다.  
    Mysql 의 인덱스 통계 정보에서 기억해야 할 점은 사용자나 DB 관리자도 모르는 상이에 통계 정보가 상당히 자주 업데이트 된다는 것이다. 가끔은 쿼리의 실행계획이 의도했던것과는 너무 다르게 만들어 질때가 있다. 이런 경우는 인덱스의 통계 정보가 실제와는 너무 다르게 수집되어 Mysql 이 실행 계획을 엉뚱하게 만들어 버리는 것이다. 이렇게 통계 정보가 크게 잘못되는 경우는 다음과 같이 자주 발생하는데, 이런 경우에는 ANALYZE 명령으로 다시 수집해 보는 것이 좋다.  
   
    ```
    - 테이블의 데이터가 별로 없는 경우(주로 개발용 데이터베이스)
    - 단시간에 대량의 데이터가 늘어나거나 줄어든 경우 
    ```
    
