---
layout: page
title: Projects
---

{% for project in site.projects_list %}
   &nbsp;&nbsp;&nbsp;<a href="{{ project[1] }}">{{ project[0] }}</a> <a href="{{ project[3] }}">{{ project[4] }}</a>
{% endfor %}
