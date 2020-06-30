---
layout  : wiki
title   : Practical vim by 드류네일 
summary : 손이 먼저 반응하는 practical vim
date    : 2020-06-29 09:35:11 +0900
updated : 2020-06-30 09:54:48 +0900
tag     : book
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 1장 Vim의 방식
점 명령을 이용해 간단한 수정 작업을 빠르게 처리한다.

## Tip1. 정 명령 만나기 
점 명령은 마지막 변경을 반복하는 기능을 수행한다.  
>G : 현재 행부터 문서 끝까지 들여쓰기를 추가하는 명령  

vim은 끼워넣기 모드로 들어간 순간부터(예를 들어 i를 눌러서) 일반모드로 돌아오기까지(<Esc>를 눌러서) 모든 입력을 기록한다.

## Tip2. 반복하지 않기
각 행의 마지막에 세미콜론을 붙이는 일은 일상에서 흔히 겪을 만한 반복 작업중한다.

tip1의 점명령을 활용해서 처리한다.
```
반값할인
C c$ : 커서 위치부터 뒤를 삭제하고 insert 모드로 진입
s cl : 선택된 단어를 삭제하고 insert 모드로 진입 
S ^C : 커서위치 상관없이 모든 줄을 삭제하고 insert 모드로 진입 
I ^i : 커서위치 상관없이 제일 앞으로 이동후 insert 모드로 진입
A $a : 커서위치 상관없이 제일 뒤로 이동후 insert 모드로 진입
o A<CR> : 아래 새로운 라인을 만들고 insert 모드로 진입
o ko :  위에 새로운 라인을 만들고  insert 모드로 진입
```
