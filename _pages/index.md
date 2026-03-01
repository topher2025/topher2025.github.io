---
layout: single
title: ""
permalink: /
---

# Jordan Alaniz

I am a junior at the Alabama School of Cyber Technology and Engineering (ASCTE) — passionate about engineering, software, and athletics. I compete on the varsity cross country and track teams, lead in clubs like CyberPatriot and National Beta Club, and spend my free time building things and optimizing systems.

→ [View Projects](/projects/)&nbsp;&nbsp;→ [Read Blog](/blog/)&nbsp;&nbsp;→ [Resume](/resume/)

---

## Recent Projects

<div class="grid__wrapper">
{% assign recent_projects = site.projects | sort: "date" | reverse %}
{% for post in recent_projects limit:4 %}
  {% include archive-single.html type="grid" %}
{% endfor %}
</div>



## Recent Posts

{% assign recent_posts = site.posts %}
{% for post in recent_posts limit:2 %}
- **[{{ post.title }}]({{ post.url }})** — *{{ post.date | date: "%B %-d, %Y" }}*  
  {{ post.excerpt | strip_html | truncatewords: 25 }}
{% endfor %}

→ [All Posts](/blog/)
