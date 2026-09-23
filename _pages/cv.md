---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* M.S. in Mathematical Engineering, KU Leuven, 2023-2025
  * Summa Cum Laude
  * Best Student of the Dutch-language version of the Master's Programme
  * including Erasmus in Sweden, Chalmers University of Technology, Fall 2023

* B.S. in Engineering Science, KU Leuven, 2020-2023
  * Cum Laude

Work experience
======
* Fall 2025 - current day : PhD Student
  * KU Leuven
  * Research and Teaching
  * Supervisor: Wim Michiels

* Summer 2024 - current day : Writing Contributor
  * Study Centre of Automatic Information Processing (SAI)
  * Covering recent developments in the tech and data science sector
  
Programming Languages
======
* MATLAB
* Python
* Julia
* R
* C
* C++
* Java
* Magma

Languages
======
* Dutch
* English
* French
* Swedish
* German

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>
  
Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>