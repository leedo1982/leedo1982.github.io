---
layout  : wiki
title   : 유니크 인덱스
summary : 
date    : 2020-11-04 06:48:49 +0900
updated : 2020-11-04 07:10:22 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 소개 
  유니크란 사실 인덱스라기 보다는 제약 조건에 가갑다고 불수 있다. 테이블이나 인덱스에 같은 값이 2개이상 저장될 수 없을을 의미하는데, NULL 도 저장 될 수 있다. 프라이머리 키는 기본적으로 NULL 을 허용하지 않는 유니크 속성이 자종으로 부연된다. 
  
# 5.10.1 유니크 인덱스와 일반 보조 인덱스의 비교 
  유니크 인덱스와 유니크하지 않은 일반 조조 인덱스는 사실 인덱스 구조상 아무런 차이점이 없다. 
  - 인덱스 읽기  
    많은 사람이 유니크 인덱스가 빠르다고 생각하지만 사실이 아니다. 유니크하지 않은 보조 인덱스는 중복된 값이 허용되므로 읽어야 할 레코드가 많아서 느린것이지, 인덱스 자체의 특성 때문에 느린것이 아니다. 
    하나의 값을 검색하는 경우, 유니크 인덱스와 일반 보조 인덱스는 사용되는 실행계획이 다르다. 하지만 이는 인덱스의 성격이 유니크한지 아닌지에 따른 차이일 뿐 큰차이는 없다. 1개의 레코드를 읽느냐 2개 이상의 레코드를 읽느냐의 차이만 있다는 것을 의미할 뿐, 읽어야 할 레코드건수가 같다면 성능상의 차이는 미비하다.  
  - 인덱스 쓰기  
    새로은 레코드가 INSERT 되거나 인덱스 칼럼의 값이 변경되는 경우에는 인덱스 쓰기 작업이 필요하다. 그런데 유니크 인덱스의 키값을 쓸때는 중복된 값이 있는지 없는지 체크하는 과정이 한단계 더 필요하다. 그로서 일반 보조 인덱스 쓰기보다 느리다. 그런데 Mysql 에서는 유니크 인덱스에서 중복된 값을 체크할 때는 읽기 잠금을 사용하고 쓰기를 할때는 쓰기 잠금을 사용하는데 이과정에서 데드락이 아주 빈번히 발생한다. 

# 5.10.2 유니크 인덱스 사용 시 주의사항 
  꼭 필요한 경우라면 유니크 인덱스를 생성하는 것은 당연하다. 하지만 더 성능이 좋아질 것으로 생각하고 불필요하게 유니크 인덱스를 생성하지는 않는 편이 좋다. Mysql 의 유니크 인덱스는 일반 다른 인덱스와 같은 역할을 하믕로 중복해서 인덱스를 생성할 필요는 없다. 그리고 가끔 똑같은 컬럼에 대해 프라이머리키와 유니크 인덱스를 동일하게 생성한 테이블도 있는데, 이또한 불필요한 중복이다. 
  