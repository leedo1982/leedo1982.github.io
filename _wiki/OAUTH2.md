---
layout  : wiki
title   : OAUTH2
summary : Open Authorization, Open Authentication
date    : 2018-06-04 10:55:56 +0900
updated : 2018-10-13 14:23:39 +0900
tags    : oauth
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# OAUTH
## OAUTH 
애플리케이션의 유저 비밀번호를 Third party 앱에 제공없이 인증, 인가를 할 수 있는 오픈 스탠다르 프로토콜

## OAuth 1.0과 OAuth 2.0 차이점
### 인증 절차 간소화
* 기능의 단순화, 기능과 규모의 확장성 등을 지원
*기존의 OAuth 1.0은 디지털 서명 기반이지만, OAuth2.0의 암호화는 https에 맡김으로 디지털 서명에 관한 로직을 요구하지 않는다.(개발자입장에서 쉬워짐)

### 용어변경
- Resource Owner : 사용자
- Resource Server : REST API 서버
- Authorization Server : 인증서버
- Cliect : 써드파치 어플리케이션

## Resource Server 와 Authorization Server 서버의 분리
- 커다란 서비스는 인증 서버를 분리하거나 다중화 할 수 있어야함.
- Authorization Server 의 역할을 명확히 함.



#### grant Type
OAuth 2.0 프레임워크의 핵심은 다양한 클라이언트 환경에 적합한 인증 및 권한의 위임 방법(grant_type)을 제공하고 그 결과로 클라이언트에게 access_token을 발급하는 것이다.


### 다양한 인증방식(Grant_type)
* Authorization Code Grant
* Implicit Grant
* Resource Owner Password Credentials Grant
* Client Credentials Grant
* Custom Validators
* JWT Profile for Client Authentication and Authorization Grants


#### Authorization Code Grant
```
+----------+
| Resource |
|   Owner  |
|          |
+----------+
     ^
     |
    (B)
+----|-----+          Client Identifier      +---------------+
|         -+----(A)-- & Redirection URI ---->|               |
|  User-   |                                 | Authorization |
|  Agent  -+----(B)-- User authenticates --->|     Server    |
|          |                                 |               |
|         -+----(C)-- Authorization Code ---<|               |
+-|----|---+                                 +---------------+
  |    |                                         ^      v
 (A)  (C)                                        |      |
  |    |                                         |      |
  ^    v                                         |      |
+---------+                                      |      |
|         |>---(D)-- Authorization Code ---------'      |
|  Client |          & Redirection URI                  |
|         |                                             |
|         |<---(E)----- Access Token -------------------'
+---------+       (w/ Optional Refresh Token)
```


#### Implicit Grant
```
+----------+
| Resource |
|  Owner   |
|          |
+----------+
     ^
     |
    (B)
+----|-----+          Client Identifier     +---------------+
|         -+----(A)-- & Redirection URI --->|               |
|  User-   |                                | Authorization |
|  Agent  -|----(B)-- User authenticates -->|     Server    |
|          |                                |               |
|          |<---(C)--- Redirection URI ----<|               |
|          |          with Access Token     +---------------+
|          |            in Fragment
|          |                                +---------------+
|          |----(D)--- Redirection URI ---->|   Web-Hosted  |
|          |          without Fragment      |     Client    |
|          |                                |    Resource   |
|     (F)  |<---(E)------- Script ---------<|               |
|          |                                +---------------+
+-|--------+
  |    |
 (A)  (G) Access Token
  |    |
  ^    v
+---------+
|         |
|  Client |
|         |
+---------+
```

#### Resource Owner Password Credentials Grant
```
+----------+
| Resource |
|  Owner   |
|          |
+----------+
     v
     |    Resource Owner
    (A) Password Credentials
     |
     v
+---------+                                  +---------------+
|         |>--(B)---- Resource Owner ------->|               |
|         |         Password Credentials     | Authorization |
| Client  |                                  |     Server    |
|         |<--(C)---- Access Token ---------<|               |
|         |    (w/ Optional Refresh Token)   |               |
+---------+                                  +---------------+
```

#### Client Credentials Grant
```
+---------+                                  +---------------+
:         :                                  :               :
:         :>-- A - Client Authentication --->: Authorization :
: Client  :                                  :     Server    :
:         :<-- B ---- Access Token ---------<:               :
:         :                                  :               :
+---------+                                  +---------------+
```






















출처 : [https://oauthlib.readthedocs.io/en/latest/oauth2/grants/grants.html ](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/grants.html )
출처 : [http://www.websphere.pe.kr/xe/new_lecture/46477 ]http://www.websphere.pe.kr/xe/new_lecture/46477 )





