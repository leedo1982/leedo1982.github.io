---
layout  : post
title   : The Difference Between SAML 2.0 and OAuth 2.0
summary : 
date    : 2018-10-09 17:55:49 +0900
updated : 2018-10-10 07:04:40 +0900
tags    : SAML Oauth
toc     : true
public  : true
---
* TOC
{:toc}

# SAML vs OAuth 
액세스 권한을 부여하는 가장 일반적인 표준 SAML 2.0과 OAuth 2.0의 차이점을 간단히 알아보자.

SAML(Security Assertion Markup Language, 샘엘)은 인증 정보 제공자(identity provider)와 서비스 제공자(service provider) 간의 인증 및 인가 데이터를 교환하기 위한 XML 기반의 개방형 표준 데이터 포맷이다  
**SAML은 SSO(Single Sign-On)를 구현하는 한 가지 방법이며, 실제 SSO는 SAML의 가장 일반적인 사용사례다**    

OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준이다  
오쓰(OAuth)는 2006년부터 구글과 트위터에서 공동으로 개발한 SAML보다 다소 새로운 표준이다. 오쓰는 모바일 플랫폼에서 SAML의 결함을 보완하기 위해 개발됐으며, XML이 아닌 JSON을 기반으로 한다.  
**SAML과는 달리 인증(Authentication)을 다루지 않는다**  

> 인증(Authentication)이란?  클라이언트가 자신이 주장하는 사용자와 같은 사용자인지를 확인하는 과정  
> 권한 부여([[Authorization]])란?  권한부여, 클라이언트가 하고자 하는 작업이 해당 클라이언트에게 허가된 작업인지를 확인  


## UseCase
![Use Case]({{ site.url }}/post-img/2018-10-09/user_case_type_standart_to_use.jpg)

> Access to applications from a potal : 포탈을 통한 애플리케이션 접근   
> Centralised identity source : 중앙집중식 ID 소스  
> Enterprise SSO : 엔터프라이즈 SSO  
> Mobile use cases : 모바일 사용 사례   
> Permanent or temporary access to resources such as accounts, files : 계정, 파일과 같은 자원에 대한 영구적 또는 일시적 액세스  


## Diff Term
![Diff Term]({{ site.url }}/post-img/2018-10-09/diff_saml_oauth_term.jpg)


## SAML FLOW
1. 사용자는 corp.sikdae.com의 공유 서비스에서 "로그인"버튼을 클릭합니다.  corp.sikdae.com의 서비스는 서비스 공급자이고 최종 사용자는 클라이언트입니다.
2. corp.sikdae.com은 사용자를 인증하기 위해 SAML 인증 요청을 구성하고 서명하고 선택 사항으로 암호화 한 다음 IdP로 직접 보냅니다.
3. 서비스 공급자는 인증을 위해 클라이언트의 브라우저를 IdP로 리디렉션합니다.
4. IdP는 수신 된 SAML 인증 요청을 확인하고 유효하면 최종 사용자가 자신의 사용자 이름과 암호를 입력 할 수있는 로그인 양식을 제공합니다.
5. 클라이언트가 성공적으로 로그인하면 IdP는 이전에 입력 한 사용자 이름과 같은 사용자 ID를 포함하는 SAML 어설 션 (SAML 토큰이라고도 함)을 생성하고이를 서비스 공급자에게 직접 보냅니다.  
IdP는 클라이언트를 다시 서비스 공급자로 리디렉션합니다.  
서비스 공급자는 SAML 어설 션을 확인하고 그로부터 사용자 ID를 추출하고 클라이언트에 대한 올바른 권한을 할당 한 다음 서비스에 로그인합니다  

![SAML Flow]({{ site.url }}/post-img/2018-10-09/SAML_flow.jpg)


## OAuth FLOW

1. 최종 사용자는 corp.sikdae.com의 서비스에서 "로그인"버튼을 클릭합니다. corp.sikdae.com의 서비스는 자원 서버이고 최종 사용자는 클라이언트입니다.
2. 자원 서버는 클라이언트에게 권한 부여를 제시하고 클라이언트를 권한 서버로 리디렉션합니다
3. 클라이언트가 권한 부여 코드를 사용하여 권한 부여 서버에 액세스 토큰을 요청합니다.
4. 클라이언트가 권한 서버에 로그인하고 코드가 유효한 경우 클라이언트는 자원 서버에서 보호 된 자원을 요청할 수있는 액세스 토큰을 얻습니다
5. 액세스 토큰이있는 보호 자원에 대한 요청을 수신 한 후
6. 자원 서버는 권한 서버와 직접 토큰의 유효성을 검증합니다
7. 토큰이 유효하면 권한 서버는 클라이언트에 대한 정보를 자원 서버로 보냅니다

![OAuth Flow]({{ site.url }}/post-img/2018-10-09/OAuth_flow.jpg)

---
출처 : [https://www.ubisecure.com/uncategorized/difference-between-saml-and-oauth ](https://www.ubisecure.com/uncategorized/difference-between-saml-and-oauth )
