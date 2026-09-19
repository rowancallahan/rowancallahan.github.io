---
layout: page
title: Archive
show_title: true
permalink: /archive/
---

<ul class="archive">
{% for post in site.posts %}
<li>{{ post.date | date: "%b %-d, %Y" }} &mdash; <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></li>
{% endfor %}
</ul>
