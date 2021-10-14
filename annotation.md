---
title: "Genome Annotation"
permalink: /annotation/
layout: single
sidebar:
  nav: sidebar-main

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Gene prediction and functional annotation is the wild west of bioinformatics"

toc: true 
toc_sticky: true
---

# Introduction 

Genome Annotation has three parts to it. First, algorithms search through the genome assembly for sequences that are known to be part of a genes structure. Second, these predicted gene sequences are compared to databases of known genes and assigned function and identities. Third, scientists manually look at the predicted genes and verify they are indeed correct.

The first two parts are relatively easy to do with a computer and software. The last part takes time and is the most difficult. For my thesis I will attempt to do the first two parts and do the third part on a few select genes that I am interested in.

Below you will find my notes, code, and other information from those attempts.



<ul>
  {% for protocols in site.annotation %}
    <li>
      <h2><a href="{{ protocols.url }}">{{ protocols.title }}</a></h2>
       </li>
  {% endfor %}
</ul>