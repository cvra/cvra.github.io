---
layout: page
title: Robots
---
{% for menu in site.data.nav %}
    {% if menu.url == '/coupe' %}
        {% for page in menu.subcategories %}
[{{ page.subtitle }}]({{ page.subhref }})
        {% endfor %}
    {% endif %}
{% endfor %}

