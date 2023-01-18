---
# Featured tags need to have either the `list` or `grid` layout (PRO only).
layout: list
type: category
title: Project
slug: project
no_groups: false
description: >
  각종 프로젝트
---
<ul>
    {% for post in site.categories[project] %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>