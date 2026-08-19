---
layout: archive
permalink: /posts/
---

{% assign entries_layout = page.entries_layout | default: 'list' %}
{% assign current_posts = site.posts | where: "era", "current" %}
{% assign high_school_posts = site.posts | where: "era", "high-school" %}

<h3 class="archive__subtitle home__section-title">Current Posts</h3>
<div class="entries-{{ entries_layout }}">
  {% for post in current_posts %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>

<h3 class="archive__subtitle home__section-title">Posts from High School</h3>
<div class="entries-{{ entries_layout }}">
  {% for post in high_school_posts %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>