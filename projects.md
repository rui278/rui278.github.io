---
layout: page
title: Projects
---

{% for project in site.projects_list %}
   &nbsp;&nbsp;&nbsp; <a href="{{ project[1] }}">{{ project[0] }}</a>
      {% for discription in site.projects_text %}
         {% if project[0] == discription[0] %}
            - {{discription[1]}};
         {% endif %}
      {% endfor %}   

{% endfor %}
