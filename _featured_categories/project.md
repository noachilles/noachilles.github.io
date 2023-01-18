---
# Featured tags need to have either the `list` or `grid` layout (PRO only).
layout: list
title: Project
slug: project
no_groups: false
description: >
  각종 프로젝트
---
<ul>
    {% for post in site.posts %}
        {% if project in post.categories %}
            <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endif %}
    {% endfor %}
</ul>