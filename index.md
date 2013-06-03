---
layout: page
title: 'Recent Posts'
---
{% include JB/setup %}

<ul class="posts recent">
  {% assign include_date = false %}
  {% assign include_author = false %}
  {% for post in site.posts limit:10 %}
    {% include themes/custom/post %}
  {% endfor %}
</ul>
