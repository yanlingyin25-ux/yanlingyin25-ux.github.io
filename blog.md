---
layout: default
title: 文章
---

<div class="page-header">
    <h1>文章</h1>
    <p style="color: #888;">记录思考、研究与系统推演</p>
</div>

<ul class="post-list">
{% for post in site.posts %}
    <li class="post-item">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        <div class="post-meta">{{ post.date | date: "%Y年%m月%d日" }}</div>
        {% if post.excerpt %}
        <div class="post-excerpt">{{ post.excerpt | strip_html | truncate: 120 }}</div>
        {% endif %}
    </li>
{% endfor %}
</ul>

{% if site.posts.size == 0 %}
<div class="container" style="text-align: center; padding: 3rem 0; color: #666;">
    <p>暂无文章，即将更新...</p>
</div>
{% endif %}
