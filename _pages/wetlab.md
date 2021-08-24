---
title: "Journal"
permalink: /wetlab/
author_profile: false
layout: archive

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Genomics is a broad field with many fascinating topics and subfields. See below for my thoughts and perspectives on them. click [here](/wetlab-categories/) to sort by category." 
---
 
{% for post in site.posts %}
  {% include archive-single.html type="grid" %}
{% endfor %}

