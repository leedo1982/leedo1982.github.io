---
layout  : wikiindex
title   : wiki
date    : 2017-11-26 21:38:36 +0900
updated : 2020-06-29 09:57:58 +0900
tags    : index
toc     : true
public  : true
comment : false
---


* [[Domain_Driven_Design]]
    * [[MODEL_DRIVEN_DESIGN]]{03.모델과 구현의 연계}
    * 제 2 부 모델 주도 셜계의 기본 요소
    * [[DOMAIN_ISOLATION]]{04.도메인의 격리}
    * [[SOFTWARE_MODEL]]{05.소프트웨어에서 표현되는 모델 }

* PROGRAMMING 
    * [[THINKING_IN_JAVA]]
    * [[EFFECTIVE_JAVA]]
    * [[EVENT-SOURCING]]
    * [[REFACTORING]]
    * [[Redis]]
    * [[git]]
    * [[Aogorism]]
    * [[Design_Pattern]]
    * [[JAVA_8]]


* FailOver
    * [[RollbackException]]  
    * [[AWS_ELB]]  


* Book
    * [[2020_BOOK_LIST]]
        * [[THE_NATURE_OF_SOFRWARE_DEVELOPEMENT]]
        * [[PRACTICAL_VIM]]
        * [[PRO_GIT]]
    * [[2019_BOOK_LIST]]

* review
    * [[2018_REVIEW]]{2018년 회고}
    * [[2018_BOOK_LIST]]

* ETC
    * [[Spring-Data-Rest]]
    * [[HAL]]
    * [[Travis_CI]]
    * [[Oauth2]]{Oauth 2.0}
    * [[SAML_OAuth]]
    * [[ACCESS_CONTROL]]{ACCESS CONTROL}
    * [[Apache_Shiro_WildcardPermission]]
    * [[primeNumber]]{왜 1은 소수가 아닌가?}
    * [[dev_flow]]{개발 프로세스}
    * [[ESC_CTRL_CAPSLOCK]]{ CAPS_LOCK 키를 ESC + CTRL 로 사용하자 }
    * [[ORM_HATE]]{OrmHate by Martim Fowler}
    * [[SPATIAL]]{Mysql 공간데이터타입}
    * [[How_to_win_friends_and_influence_people]]{인생의 행동목표}

---

# blog
<div>
    <ul>
{% for post in site.posts %}
    {% if post.public != false %}
        <li>
            <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                {{ post.title }}
            </a>
        </li>
    {% endif %}
{% endfor %}
    </ul>
</div>

