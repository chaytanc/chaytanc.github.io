---
layout: page
title: Portfolio
nav_order: 2
permalink: /portfolio/
description: A collection of my work across AI, political analysis, leadership, and more.
---

# Portfolio
{: .no_toc}

A collection of projects spanning Political Analysis, AI & Research, Leadership, and Fun. Click on any card to learn more!

## Categories

<div class="portfolio-grid">
    <a href="#political-analysis" class="category-card">
        <h3>Political Analysis</h3>
        <p>Research and projects analyzing governance, geopolitics, and policy.</p>
    </a>
    <a href="#ai-research" class="category-card">
        <h3>AI & Research</h3>
        <p>Explorations in machine learning, natural language processing, and neuroscience.</p>
    </a>
    <a href="#leadership" class="category-card">
        <h3>Leadership</h3>
        <p>Initiatives, advocacy, and community projects.</p>
    </a>
    <a href="#fun" class="category-card">
        <h3>Fun</h3>
        <p>Creative personal projects and experiments.</p>
    </a>
</div>

---

## Political Analysis {#political-analysis}

{% for project in site.projects %}
{% if project.category == "Political Analysis" %}
<div class="project-card">
    <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
    <p>{{ project.description }}</p>
</div>
{% endif %}
{% endfor %}

---

## AI & Research {#ai-research}

{% for project in site.projects %}
{% if project.category == "AI & Research" %}
<div class="project-card">
    <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
    <p>{{ project.description }}</p>
</div>
{% endif %}
{% endfor %}

---

## Leadership {#leadership}

{% for project in site.projects %}
{% if project.category == "Leadership" %}
<div class="project-card">
    <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
    <p>{{ project.description }}</p>
</div>
{% endif %}
{% endfor %}

---

## Fun {#fun}

{% for project in site.projects %}
{% if project.category == "Fun" %}
<div class="project-card">
    <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
    <p>{{ project.description }}</p>
</div>
{% endif %}
{% endfor %}
