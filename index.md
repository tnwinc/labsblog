---
layout: page
title: 'Recent Posts'
---
{% include JB/setup %}

<ul class="posts recent">
  {% for post in site.posts limit:10 %}
    <li>
      <a href="{{ BASE_PATH }}{{ post.url }}" title="{{ post.title }}">
        <h2>{{ post.title }}</h2>
        <p>{{ post.summary }}</p>
      </a>
    </li>
  {% endfor %}
</ul>
