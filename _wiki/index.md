---
layout  : wikiindex
title   : wiki
date    : 2017-11-26 21:38:36 +0900
updated : 2020-12-22 07:40:49 +0900
tags    : index
toc     : true
public  : true
comment : false
---



* Book
    * [[2020_BOOK]]
        * [[REAL_MYSQL]]

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

