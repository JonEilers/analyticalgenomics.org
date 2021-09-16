---
title: "Sequence Data Analysis and QC"
permalink: /data-cleaning/
layout: collection
sidebar:
  nav: sidebar-main
classes: wide
entries_layout: grid

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Garbage in, garbage out."
---
 Each sequencing technology introduces it own unique artifacts and inaccuracies into the sequencing data. Identifying, analyzing, and removing these artifacts is the first step in preparing the sequence data for assembly. Below you will find the code and results for Illumina Hiseq X and Oxford Nanopore Technologies MinION sequencing of the *Apostichopus californicus* genome.
    

<ul>
  {% for protocols in site.data-cleaning %}
    <li>
      <h2><a href="{{ protocols.url }}">{{ protocols.title }}</a></h2>
       </li>
  {% endfor %}
</ul>


