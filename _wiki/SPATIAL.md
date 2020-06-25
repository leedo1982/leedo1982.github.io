---
layout  : wiki
title   : SPATIAL
summary : 공간 데이터타입
date    : 2018-05-28 13:21:22 +0900
updated : 2018-05-28 13:51:31 +0900
tags    : mysql
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# MYSQL 공간데이터 타입
 GPS 를 이용한 위치 정보를 포함해 모든 좌표 형식의 데이터를 관리할 수 있는 개념

## POINT 
가장 기본적인 데이터 타입이로 x와 y의 좌표만으로 구성된 하나의 점정보를 저장할 수 있는 데이터 타입
POINT(x,y), GeomFromText('Point(x,y)')로 데이터 생성가능

## LINESTRING
하나의 직선 뿐 아니라 여러개의 꺽임이 있는 연결된 선도 모두 저장 가능
LINESTRUNG(), LintStringFromText() 로 데이터 생성가능

## POLYGOM
다각형(시작과 끝지점이 일치하는 닫히 도형)을 저장할 수 있는 데이터 타입

## GEOMETRY
위의 모든 데이터 타입을 모두 포함하는 슈퍼타입. GIS 처럼 다양한 공간 정보를 저장하는 칼럼이 필요할때는 점이나 라인뿐 아니라 복잡하고 다양한 다각형 데이터가 담기는데 이때 이용하면 된다.

## function
### st_x(geom), st_y(geom)
```
SELECT ST_X(geom),ST_Y(geom) FROM restaurant ;
```
point의 X과 Y축을 구해준다.

### st_distance_sphere
```
select st_distance_sphere(POINT(-73.9949,40.7501), POINT( -73.9961,40.7542)) ;
```
두 지점사이의 거리를 구해준다.

## 용어정리
### WGS84
한 지점의 좌표 값은 어떤 측지계를 기준으로 하느냐에 따라 달라진다. 
과거 우리나라에서 사용하던 측지계는 Tokyo를 중심으로 사용했다. 
최근에는 WGS84(1984년에 만든 최신 Word Geodetic System 이다. 
WGS 1984 혹은 EPSG:4326이라고 부르기도 한다)라고 부르는 세계측지계를 사용한다.
WGS84의 좌표 원점은 지구 질량 중심에 있다. 위치는 2cm 보다 작다고 한다.

# 참조
[SPATIAL](https://www.joinc.co.kr/w/man/12/spatial )
[MySQL Spatial Data Types](https://www.w3resource.com/mysql/mysql-spatial-data-types.php )
[ Real MySQL 15장 데이터 타입 ](http://casek.tistory.com/entry/Real-MySQL-15%EC%9E%A5-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%83%80%EC%9E%85 )
[12.15.7.1 General Geometry Property Functions](https://dev.mysql.com/doc/refman/5.7/en/gis-general-property-functions.html )











