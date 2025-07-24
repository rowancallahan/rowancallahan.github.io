---
layout: page
title: Archive
permalink: /archive/
---

## All Posts

<div class="archive-container">
{% for post in site.posts %}
<div class="archive-post">
<span class="archive-date">{{ post.date | date: "%B %-d, %Y" }}</span>
<h3><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h3>
{% if post.excerpt %}
<p class="archive-excerpt">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
{% endif %}
</div>
{% endfor %}
</div>