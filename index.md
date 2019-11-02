---
layout: page
title: Blog Posts
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

# Publications
* 15 Jun 2018 &raquo; [Blockchain: A Game Changer](https://dzone.com/articles/blockchain-the-business-game-changer)
* 01 Jul 2018 &raquo; [Majestic Disorder, Issue 10, Featured Minds](http://majesticdisorder.com/issues-inside-issue-10/)
* 01 Jul 2016 &raquo; [Majestic Disorder, Issue 7, Featured Minds](http://majesticdisorder.com/featured-minds-of-issue-7/)
* 24 Jun 2015 &raquo; [Behind the lens: Rainbow Rights](https://www.photocrowd.com/blog/94-behind-lens-rainbow-rights/)