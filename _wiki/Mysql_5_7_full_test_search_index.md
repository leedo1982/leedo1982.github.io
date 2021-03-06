---
layout  : wiki
title   : Full Text search 인덱스
summary : 
date    : 2020-11-02 07:42:25 +0900
updated : 2020-11-02 08:12:02 +0900
tag     : 
toc     : true
public  : true
parent  : [[REAL_MYSQL]]
latex   : false
---
* TOC
{:toc}

# 소개 
  지금까지 앞서 설명한 인덱스 알고리즘은 일반적으로 크지 않은 데이터 또는 이미 키워드화돼 있는 작은 값에 대한 인덱싱 알고리즘이다. 
  문서의 내용 전체를 인덱스화해서 특정 키워드가 포함된 문서를 검색하는 전문검색에는 B-Tree 를 사용할수 없다. 문서 전체에 대한 분석과 검색을 위한 이러한 인덱싱 알고리즘을 전문검색(Full Text search) 인덱스라고 하는데, 일반화된 기능의 명칭이지 전문 검색알고리즘을 지칭하는것은 아니다. 전문검색 알고리즘은 인덱싱하는 방법에 따라 구분자(stopword)와 N-그램으로 나눠서 생각해볼 수 있다.
  
# 5.7.1 인덱스 알고리즘 
  Mysql 의 모든 버전에서 기본적으로 제공하는 전문검색 엔진의 인덱스 방식인 구분자와 Mysql 이 아닌 서드파티에서 제공하는 전문검색기능에서 주로제공하는 N-그램 방식에 대해 살펴보겠다. 
  
## 구분자(Stopword) 기법  
  전문의 내용을 공백이나 탭 또는 마침표와 같은 문장 기호, 그리고 사용자가 정의한 문자열을 구분자로 등록한다. 구분자 기법은 이처럼 등록된 구분자를 이용해 키워드를 분석해내고, 결과 단어를 인덱스로 생성해두고 검색에 이용하는 방법을 말한다. 
  구분자 기법은 문서의 본문으로부터 키워드를 추출해 내는 작업이 추가로 필요할 뿐, 내부적으로는 B-Tree 인덱스를 그대로 사용한다. 
  
## N-그램(n-Gram) 기법 
  하지만 각 국가의 언어는 띄어쓰기가 전혀 없다거나 문장기호가 전혀 다른 경우가 허다하다. 이러한 부분을 보완하기 위해 지정된 규칙이 없는 전문도 분석 및 검색을 가능하게 하는 방법이 N-그램이라는 방식이다. 
  N-그램이란 본문을 무조건적으로 몇 글자씩 잘라서 인덱싱하는 방법이다. 구분자에 의한 벙법보다는 인덱싱 알고리즘이 복잡하고, 만들어진 인덱스의 크기도 상당히 큰편이다. N-그램에서 n은 인덱싱할 키워드의 최소글자 수를 의미하는데, 일반적으로 2글자 단위로 키워드를 쪼개서 인덱싱하는 2-Gram 방식이 많이 사용된다. 
  2-Gram 인덱싱 기법은 2글자 단위의 최소 키워들에 대한 키를 프론트엔드 인덱스와 2글자 이상의 키워드 묶음을 관리하는 백엔드 인덱스 2개로 구성된다. 
  ```
   첫번째 단계로, 문서의 본문을 2글자보다 큰 크리고 블록을 구분해서 백엔드 인덱스를 생성
   두번째 단계로, 백엔드 인덱스의 키워드들은 2글자씩 잘라서 프론트 인덱스를 생성 
  ```

# 5.7.2 구분자와 N-그램의 차이 
  실제 사용자가 보는 가장큰 차이는 검색결과에 있다. 구분자 방식의 검색에서는 반드시 구분자를 기준삼아 외쪽 일치 기준으로 비교검색을 실행한다. 따라서 검색어가 '아이폰'일경우 '애플아이폰'은 찾아낼 수 없다. 또한 N-그램 전문 검색 인덱스는 구분자 방식보다 인덱스의 크기가 크다.
  
# 5.7.3 전문 검색 인덱스의 가용성 
  전문 검색 인덱스를 사용하려면 반드시 그에 맞는 구문을 사용해야 한다. 전문검색시 반드시 MATCH(...) AGAINST(...) 구문으로 검색 쿼리를 작성해야 하며, MATCH 절의 괄호에 포함되는 내용은 반드시 사용할 전문 검색 인덱스에 정의된 칼럼이 모두 명시돼야 한다. 
