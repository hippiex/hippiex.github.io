---
layout: page
title: Categories
permalink: /categories/
summary: "A list of all my posts by category."
---

<div class="links-list">
{% assign tags_list = site.categories %}
  {% if tags_list.first[0] == null %}
    {% for tag in tags_list %}
      <a href="#{{ tag }}">{{ tag | capitalize }} <span> | {{ site.tags[tag].size }}</span></a>
    {% endfor %}
  {% else %}
    {% for tag in tags_list %}
      <a href="#{{ tag[0] }}">{{ tag[0] | capitalize }} <span> | {{ tag[1].size }}</span></a>
    {% endfor %}
  {% endif %}
{% assign tags_list = nil %}
</div>

<div class="clear"></div>
{% for tag in site.categories %}
  <h2 id="{{ tag[0] }}">{{ tag[0] | capitalize }}</h2>
  <ul class="post-list">
    {% assign pages_list = tag[1] %}
    {% for post in pages_list %}
      {% if post.title != null %}
      {% if group == null or group == post.group %}
      <li>
		<a href="{{ site.url }}{{ post.url }}">{{ post.title }} | <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></span></a></li>
      {% endif %}
      {% endif %}
    {% endfor %}
    {% assign pages_list = nil %}
    {% assign group = nil %}
  </ul>
{% endfor %}

