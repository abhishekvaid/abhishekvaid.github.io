---
layout: single  # Make sure you are using the correct layout
author: vaidabhishek
permalink: /leadership/
---

<h1>Management & Leadership</h1>

{% assign posts = site.posts | where: "categories", "leadership" %}
{% assign grouped_posts = posts | group_by: "year" %}

{% for year in grouped_posts %}
<h2>{{ year.name }}</h2>
<ul>
    {% assign month_posts = year.items | group_by: "month" %}
    {% for month in month_posts %}
    <h3>{{ month.name }}</h3>
    <ul>
        {% for post in month.items %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endfor %}
    </ul>
    {% endfor %}
</ul>
{% endfor %}
