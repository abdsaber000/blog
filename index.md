---
layout: default
title: Blog
permalink: /
---

{% if site.posts.size == 0 %}
<p class="empty-state">No notes yet. Add a file under <code>_posts/</code>.</p>
{% else %}
<ul class="post-list">
  {% for post in site.posts %}
  <li>
    <a class="title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
    <p class="meta">
      <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %-d, %Y" }}</time>
      {% if post.tags and post.tags.size > 0 %}
        · {% for tag in post.tags %}<span class="tag">{{ tag }}</span>{% unless forloop.last %} {% endunless %}{% endfor %}
      {% endif %}
    </p>
    {% if post.excerpt %}
    <p class="excerpt">{{ post.excerpt | strip_html | truncate: 160 }}</p>
    {% endif %}
  </li>
  {% endfor %}
</ul>
{% endif %}
