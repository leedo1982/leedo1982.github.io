---
layout  : wiki
title   : InnoDB 스토리지 엔진 잠금
summary : 
date    : 2020-10-13 07:16:27 +0900
updated : 2020-10-19 08:05:30 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 소개 
  Mysql 에서 제공하는 잠금과는 별개로 스토리지 엔진 내부에서 레코드 기반의 잠금 방식을 탑재하고 있다. 이때문에 MyISAM 보다 훨씬 뛰어난 동시성 처리를 제공할 수 있다. 
  
# 4.4.1 InnoDB 잠금 방식 
  >비관적 잠금: 현재 트랜잭션에서 변경하고자 하는 레코드에 대해 잠금을 획득하고 변경작업을 처리하는 방식. 이처럼 다른 트랜잭션도 변경할 수 있다는 비관적인 가정으하기 때문에 먼저 잠금을 획들하는 것이다. InnoDB 는 비관적 잠금 방식을 채택하고 있다.
  >낙관적 잠금: 각 트랜잭션이 같은 레코드를 변경할 가능성은 상당희 희박할 것이라고 가정한다. 그래서 우선 변경작업을 수행하고 마지막에 잠김 충돌이 있었는지 확인해 문제가 있었다면 ROLLBACK 처리하는 방식을 의미한다.
  
# 4.4.2 InnoDB 잠금 종류 
  레코드 기반의 잠금 기능을 제공하며, 잠금 정보가 상당히 작은 공간으로 관리되기 때문에 리코드 락이 페이지 락으로 또는 테이블 락으로 레벨업되는 경우는 없다.
  >레코드락: 레코드 자체만을 잠그는 것을 레코드락이라고하며, 다른 상용 DBMS 의 레코드락과 동일한 역할을 한다.
  >갭락: 다른 DBMS 와의 또 다른 차이가바로 갭락이다. 갭락은 레코드 그 자체가 아니라 레코드와 바로 인적한 레코드 사이의 간격만을 잠그는 것을 의미한다.
  >넥스트키락: 레코드 락과 갭락을 합쳐놓은 형태의 잠금을 넥스트키락이라고 한다.
  >자동증가락: AUTO_INCREMENT 컬럼이 사용된 테이블에 동시에 여러 레코드가 INSERT 되는경우, 저장되는 각레코드는 중복되지 않고 저장된 순서대로 증가한 일련번호 값을 가져야 한다. InnoDB 스토리지 엔진에서는 이를위해 내부적으로 자동증가락이라는 테이블 수준의 잠금을 사용한다.
  
# 4.4.3 인덱스와 잠금 
  
  ```
  mysql> SELECT COUNT(*) FROM employees WHERE first_name = 'Georgi'; // 253
  mysql> SELECT COUNT(*) FROM employees WHERE first_name = 'Georgi' AND last_name ='Klassen'; // 1
  mysql> Update employees SET hire_Date=NOW() WHERE first_name = 'Georgi' AND last_name ='Klassen';
  ```
  update 문을 실행하면 1건의 레코드가 업데이트 될 것이다. 몇개의 레코드에 락이 걸릴까?
  first_name 에는 인덱스가 있고, last_name 에는 인덱스가 없기때문에 first_name 인 레코드 253개가 모두 잠긴다. 만약 인덱스가 하나도 없다면 테이블을 풀 스캔하면서 update 작읍을 하는데, 이과정에서 테이블에 있는 30여 만건의 모든 레코드를 잠기게 된다.
  
# 4.4.4 트랜잭션 격리 수준과 잠금 
  불필요한 레코드의 잠금 현상을 InnoDB 의 넥스트 키 락 때문에 발생하는 것이지만, 주원인은 복제를 위한 바이너리 로그 때문이다. 다음 조합으로 서버가 기동할 경우 InnoDB 에서 사용되는 대부ㅜ분의 갭락이나 넥스트키 락을 제거할수 있다.
  ```
  Mysql 5.1 이상 : 바이너리 로그 비활성화 / 트랜잭션 격리수준을 READ-COMMITTED로 설정
                   레코드 기반의 바이너리 로그 사용 / innodb_locks_unsafe_for_binlog=1/ 트랜잭션 격리수준을 READ-COMMITTED로 설정
  ```
  
  위 조합으로 설정되면 4.4.3의 불필요한 잠금도 일부 없어 진다. UPDATE 문장으로 처리하기 위해 일치하는 레코드르 인덱스를 이용해 검색할때, 우선 인덱스만을 비교해서 일치하는 레코드에 대해 배타적 잠금을 걸게 되지만, 그 다음 나머지 조건을 비교해서 일치하지 않는 레코드는 즉시 잠그을 해제한다.
  
# 4.4.5 레코드 수준의 잠금 확인 및 해제 
  더 자세한 각 커넥션의 트랜잭션 상황을 살펴보기 위한 "SHOW ENGINE INNODB STATUS"
  ```
  mysql> SELECT 
  r.trx_id waiting_trx_id,
  r.trx_mysql_thread_id waiting_thread,
  r.trx_query waiting_query,
  rb.trx_id blocking_trx_id,
  b.trx_mysql_thread_id blocking_thread,
  b.trx_query blocking_query
  FROM iformation_schema.innodb_lock_watis w
  INNER JOIN  information_schema.innodb_trx b ON b.trx_id = w.blocking_trx_id
  INNER JOIN information_schema.innodb_trx r ON r.trx_id = w.requesting_trx_id

  ```
  
  
# Mysql 의 격리 수준 
  트랜잭션 격리 수준이란 동시에 여러 트랜잭션이 처리될때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼수 있도록 허용할지 말지 결정하는 것이다.
  
  ---
| 구분             | DIRTY READ    | NON-REPEATABLE READ | PHANTOM READ               |
| READ UNCOMMITTED | 발생          | 발생                | 발생                       |
| READ COMMITTED   | 발생하지 않음 | 발생                | 발생                       |
| REPEATABLE READ  | 발생하지 않음 | 발생하지 않음       | 발생(innoDB 발생하지 않음) |
| SERIALIZABLE     | 발생하지 않음 | 발생하지 않음       | 발생하지 않음              |
  
  ---
  
