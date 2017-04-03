---
layout: page
title: C++
excerpt: ""
search_omit: true
---

All the posts here are summarized/cited from the other sources. It's mainly a record and small summary about what/when I read the resources, thanks for the abundant information on the Internet/books.

Hope I do not miss any of the references at the beginning. If I did, or posted something that violated your rights, please contact me through `scuendless@gmail.com`.

---

<ul class="post-list">
{% for post in site.categories.cpp %}
  <li><article><a href="{{ site.url }}{{ post.url }}">{{ post.title }} <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span>{% if post.excerpt %} <span class="excerpt">{{ post.excerpt | remove: '\[ ... \]' | remove: '\( ... \)' | markdownify | strip_html | strip_newlines | escape_once }}</span>{% endif %}</a></article></li>
{% endfor %}
</ul>
