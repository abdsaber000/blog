---
layout: page
title: Videos
permalink: /videos/
---

YouTube videos. Add an entry in `_data/videos.yml` with the video id from the URL (`youtube.com/watch?v=VIDEO_ID`).

{% if site.data.videos and site.data.videos.size > 0 %}
<ul class="item-list">
  {% assign items = site.data.videos | sort: "date" | reverse %}
  {% for item in items %}
  <li>
    <p class="item-title">{{ item.title }}</p>
    <p class="item-meta">{% if item.date %}{{ item.date | date: "%B %-d, %Y" }}{% endif %}</p>
    {% if item.description %}
    <p class="item-desc">{{ item.description }}</p>
    {% endif %}
    {% if item.youtube_id and item.youtube_id != "" %}
    <div class="video-embed">
      <iframe
        src="https://www.youtube-nocookie.com/embed/{{ item.youtube_id }}"
        title="{{ item.title }}"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        loading="lazy"></iframe>
    </div>
    {% endif %}
  </li>
  {% endfor %}
</ul>
{% else %}
<p class="empty-state">No videos yet — coming soon.</p>
{% endif %}
