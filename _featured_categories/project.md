---
# Featured tags need to have either the `list` or `grid` layout (PRO only).
layout: list
title: Project
slug: project
no_groups: true
description: >
  각종 프로젝트
---

<ul>
    {% for post in site.categories.project %}
        {% if post.url %}
            <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% else %}
            there is no contents.
        {% endif %}
    {% endfor %}
</ul>