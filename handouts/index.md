---
layout: page
title: Handouts
permalink: /handouts/
---

PDF notes, solutions, and other handouts. Drop a file in `assets/handouts/` and add an entry in `_data/handouts.yml`.

{% if site.data.handouts and site.data.handouts.size > 0 %}
<ul class="item-list">
  {% assign items = site.data.handouts | sort: "date" | reverse %}
  {% for item in items %}
  <li>
    <p class="item-title">
      <a href="{{ item.file | relative_url }}">{{ item.title }}</a>
    </p>
    <p class="item-meta">
      {% if item.date %}{{ item.date | date: "%B %-d, %Y" }}{% endif %}
      {% if item.date %} · {% endif %}
      <a href="{{ item.file | relative_url }}">PDF</a>
    </p>
    {% if item.description %}
    <p class="item-desc">{{ item.description }}</p>
    {% endif %}
  </li>
  {% endfor %}
</ul>
{% else %}
<p class="empty-state">No handouts yet.</p>
{% endif %}
