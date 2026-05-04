---
layout: page
title: Posts by Category
permalink: /categories/
---

<div id="home">
<!-- <h1>Posts by Category</h1> -->
{% assign rendered_categories = "|" %}

{% if site.category_order %}
  {% for ordered_category in site.category_order %}
    {% assign ordered_posts = site.categories[ordered_category] %}
    {% if ordered_posts %}
      <h3>{{ ordered_category }}</h3>
      <ul>
        {% for post in ordered_posts %}
          <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a>
          <p> {{ post.description }} </p>
          </li>
        {% endfor %}
      </ul>
      {% assign rendered_categories = rendered_categories | append: ordered_category | append: "|" %}
    {% endif %}
  {% endfor %}
{% endif %}

{% assign sorted_categories = site.categories | sort_natural %}
{% for category in sorted_categories %}
  {% assign category_name = category[0] %}
  {% assign category_token = "|" | append: category_name | append: "|" %}
  {% unless rendered_categories contains category_token %}
    <h3>{{ category_name }}</h3>
    <ul>
      {% for post in category[1] %}
        <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a>
        <p> {{ post.description }} </p>
        </li>
      {% endfor %}
    </ul>
  {% endunless %}
{% endfor %}
</div>