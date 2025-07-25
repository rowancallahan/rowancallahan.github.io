---
layout: page
title: Archive
show_title: true
permalink: /archive/
---


<div class="archive-container">
{% for post in site.posts %}
<div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; padding: 4px 0;">
<span class="archive-date" style="font-size: 0.9em; color: #666; min-width: 120px;">{{ post.date | date: "%b %-d, %Y" }}</span>
<a href="{{ post.url | relative_url }}" style="text-decoration: none; flex-grow: 1; margin-left: 20px;">{{ post.title | escape }}</a>
</div>
{% endfor %}
</div>
