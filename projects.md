---
layout: page
title: Projects
---

{% for project in site.projects_list %}
   &nbsp;&nbsp;&nbsp;<a href="{{ project[1] }}">{{ project[0] }}</a>{{project[3]}}
{% endfor %}
