---
layout: page
title: Projects
---

# Projects

{% for project in site.projects_list %}
    * &nbsp;&nbsp;&nbsp;<a href="{{ page[1] }}">{{ page[0] }}</a>
{% endfor %}
