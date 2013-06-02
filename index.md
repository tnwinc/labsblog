---
layout: page
title: 'Recent Posts'
---
{% include JB/setup %}

<ul class="posts recent">
  {% for post in site.posts limit:10 %}
    <li class="post-in-list {% cycle 'blue', 'green', 'orange' %}">
      <a href="{{ BASE_PATH }}{{ post.url }}" title="{{ post.title }}">
        <span class="molecule-{% cycle 'caffeine', 'adrenaline', 'dopamine', 'acetaminophen' %}">&nbsp;</span>
        <h2>{{ post.title }}</h2>
        <p>{{ post.summary }}</p>
      </a>
    </li>
  {% endfor %}
</ul>
