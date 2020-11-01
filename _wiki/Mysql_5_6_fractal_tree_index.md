---
layout  : wiki
title   : Fractal-Tree 인덱스 
summary : 
date    : 2020-11-02 07:25:32 +0900
updated : 2020-11-02 07:41:51 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 소개 
  Fractal-Tree 는 최근에 개발된 기술인데, 안타깝게도 독점적인 특허로 등록된 알고리즘 이어서 아직많은 DBMS 에서 구현되지 못하고 있다. Mysql의 스토리지 엔진인 TokuDB에만 적용돼 있다. 결론적으로 대용량으로 변해가는 DBMS 업계에서 어느 정도의 해결책을 제시해 줄 인덱싱 알고리즘으로 생각된다.
  
# 5.6.1 Fractal-Tree 특징 
  B-Tree 인덱스에서 인덱스 키를 검색하거나 변경하는 과정 중에 발생하는 가장 큰 문제는 디스크의 랜덤 I/O가 상대적으로 많이 필요하다는 것이다. Fractal-Tree 는 이러한 B-Tree 의 단점을 최소화하고, 이를 순차 I/O로 변환해서 처리할 수 있다는 것이 가장 큰 장점이다. 
  Fractal-Tree 는 인덱스 키가 추가되거나 삭제될 때 B-Tree인덱스보다 더 많은 정렬 작업이 필요하며, 이때문에 더많은 CPU 처리가 필요할지 모른다. 하지만 인덱스의 단편화가 발생하지 않도록 구성할 수 있고, 인덱스의 키값을 클러스터링 하기 때문에 B-Tree 보다는 대용량 테이블에서 높은 성능을 보장한다. 
  Fractal-Tree 의 평균적인 처리 능력은 이론적으로B-Tree 보다 400배 가량 빠르며, 실제로도 TokuDB 의 키 추가 및 삭제작업은 InnoDB보다 100배 가량 빠른 처리 속도를 보여준다. TokuDB의 동시성처리가 안정적으로 구현된다면 Mysql에서 tokuDB의 위상은 상당히 높아질 것이다.
  
# 5.6.2 Fractal-Tree 의 가용성과 효율성 
  Fractal-Tree 의 또다른 장점은 B-Tree 의 장점을 그대로 가지고 있다는 것이다. 
  앞으로 TokuDB 가 어떻게 변화해 갈지는 모르겠지만 지금의 TokuDB 는 동시성이 떨어지기 때문에 웹서비스 같은 OLAP 환경에는 아직 적용하기에 무리가 있다. 

