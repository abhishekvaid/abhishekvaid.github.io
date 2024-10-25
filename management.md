---
layout: default  # Make sure you are using the correct layout
title: "Management"
permalink: /management/
---

<h1>Management Articles</h1>

{% assign management_posts = site.posts | where: "categories", "management" %}
{% assign posts_by_year = management_posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in posts_by_year %}
  <h2>{{ year.name }}</h2>
  {% assign posts_by_month = year.items | group_by_exp: "post", "post.date | date: '%B'" %}
  {% for month in posts_by_month %}
    <h3>{{ month.name }}</h3>
    <ul>
      {% for post in month.items %}
        <li><a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: '%Y-%m-%d' }}</li>
      {% endfor %}
    </ul>
  {% endfor %}
{% endfor %}
