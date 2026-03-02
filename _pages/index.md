---
layout: home
title: ""
permalink: /
author_profile: true
feature_row:
  - title: "Projects"
    excerpt: "Engineering and software projects I've built and documented."
    url: "/projects/"
    btn_label: "View Projects"
    btn_class: "btn--primary"
  - title: "Blog"
    excerpt: "Short technical notes on algorithms, hardware, and design."
    url: "/blog/"
    btn_label: "Read Blog"
    btn_class: "btn--primary"
  - title: "Resume"
    excerpt: "My academic background, skills, and experience."
    url: "/resume/"
    btn_label: "View Resume"
    btn_class: "btn--primary"
---

## About Me

I am a junior at the Alabama School of Cyber Technology and Engineering (ASCTE) — passionate about engineering, software, and athletics. I compete on the varsity cross country and track teams, lead in clubs like CyberPatriot and National Beta Club, and spend my free time building things and optimizing systems.

{% include feature_row %}

<h2 class="archive__subtitle">Recent Projects</h2>

{% assign recent_projects = site.projects | sort: "date" | reverse %}
{% for post in recent_projects limit:2 %}
  {% include archive-single.html %}
{% endfor %}

