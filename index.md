---
layout: page
title: Blog
---
<div id="home">
  <ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a>
      <p> {{ post.description }} </p>
      </li>
    {% endfor %}
  </ul>
</div>
