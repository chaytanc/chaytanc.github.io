---
layout: home
title: About
nav_order: 1
nav_exclude: false
permalink: /
description: More than you wanted to know 
---

# About
{:.no_toc}
More than you wanted to know
{: .fs-6 .fw-300 }


[comment]: # (## Table of contents)

[comment]: # (1. TOC)

[comment]: # (   {:toc})

{: .no_toc .text-delta }


---

{% assign me = site.staffers | where: 'role', 'Me' %}
{% for staffer in me %}
{{ staffer }}
{% endfor %}

Born in Portland. Spent much of my youth having a bad time in school. Went off to Beaverton High School.
Very grateful to my experiences in the entirety of my education so far, good and bad.
Now a student at the University of Washington studying Computer Science.
Have worked in several research labs, the first being Adey Lab at OHSU and the second being 
Freedman Lab at the Institute for Stem Cell and Regenerative Medicine at UW. I have grown many 
things but one of my favorites was the organoids I grew at Freedman Lab.
Robotics in high school also had an outsized impact on me. 
Now I run an undergraduate research group called the Interactive Intelligence team 
which applies neuroscience to machine learning 
in the pursuit of creating intelligent machines. I am very proud of the team [which you can find here.](https://interactive-intelligence.github.io){:target="_blank"}