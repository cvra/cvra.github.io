---
layout: page
title: Technologies
---
<div class="album">
{% for menu in site.data.nav %}
    {% if menu.url == '/technologies' %}
        {% for page in menu.subcategories %}
            {% cycle '<div class="row">', '' %}
                <a href="{{ page.subhref }}">
                    <div class="large-6 columns thumbnail">
                        <img src="{{ page.imgref }}" alt="{{ page.subtitle }}">
                        <h3>
                            <br>
                            <span>
                                {{ page.subtitle }}
                            </span>
                        </h3>
                    </div>
                </a>
            {% cycle '', '</div>' %}
        {% endfor %}
    {% endif %}
{% endfor %}
