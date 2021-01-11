---
layout  : wikiindex
title   : wiki
date    : 2017-11-26 21:38:36 +0900
updated : 2021-01-11 10:12:19 +0900
tags    : index
toc     : true
public  : true
comment : false
---



* Book
    * [[2021_BOOK]]
        * [[REAL_MYSQL]]
        * [[DOMAIN_DRIVE_DESIGN_DISTILLED]]{ 도메인 주도설계 핵심 }
        * ELEGANT_OBJECT(엘레강트 오브젝트)
        * 


* WIP
    * [[payment_server_legacy]]{결제와 관련없는 결제서버 레거시 개선이야기  }

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

