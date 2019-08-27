---
layout: post
title:  "Resource list"
categories: mainpage
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

* [About](about)
