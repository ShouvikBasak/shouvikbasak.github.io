---
layout: page
title: Posts by Category
permalink: /categories/
---

<div id="home">
<!-- <h1>Posts by Category</h1> -->
{% for category in site.categories % | sort %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a>
      <p> {{ post.description }} </p>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
</div>