---
nav_order: 3
nav_exclude: false
permalink: /projects
layout: page
title: Projects

description: Listing of some of the projects I have made

---

# Projects
#### Some of the projects I have coded or written recently

[ModelVale](/ModelVale)  
[Eye and Mind](/eyeandmind)   
[Game of Intelligent Life](/gil)  
[Cybernetic Fungi](/fungi-cy)


Total projects found: **{{ site.projects | size }}**  
{% for project in site.projects %}
- [{{ project.title }}]({{ project.url }}) - {{ project.category }}
{% endfor %}
