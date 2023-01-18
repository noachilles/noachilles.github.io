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
    {% for category in site.categories.project %}
        {% if category.last %}
            <li><a href="{{ post.url }}">{{post.title}}</a></li>
        {% endif %}
    {% endfor %}
</ul>