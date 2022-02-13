---
title: "Genome Bioinformatics"
permalink: /bioinformatics/
author_profile: false
sidebar:
  nav: sidebar-main
classes: wide
entries_layout: grid

feature_row:
  - image_path: /assets/images/bioinformatics_page/datacleaning.jpg
    title: "Data Cleaning"
    url: "/data-cleaning/"
  - image_path: /assets/images/bioinformatics_page/genomeassembly.png
    title: "Genome Assembly"
    url: "/assembly/"
  - image_path: /assets/images/bioinformatics_page/geneannotation.jpg
    title: "Genome Annotation"
    url: "/annotation/"

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "A gentle introduction to genome assembly and annotation"
---

{% include feature_row %}

# Genome Bioinformatics

Genomes do not spontaneously create themselves. At least in silico. Once raw sequence data is acquired, quality and content is checked. Then an assembly tool is selected and run. When assembled, the assembly quality is assessed. If it's "good enough" repetitive element masking and gene prediction are performed. If everything checks out afterwards, the genome is ready for interrogation. Above are three pages containing content on the process for each of these steps. 

# A little background 

This website is intended to be a gentle introduction to genome assembly and annotation. While I will occasionally go into detail regarding how things work, the algorithms and software design of these tools are complex and beyond the scope of this website. I hope this can be a good starting point for those who are interested though. 

In the upcoming years genome assembly is going to explode. There are millions of species and many more individuals of those species that need their genomes sequenced and we are beginning to have the opportunity to do so. This will become especially true once long read sequencing becomes extraordinarily cheap with the advent of solid state nanopore sequencing. However, the tools and knowledge to do so are scattered across scientific papers, github repositories, and academic institutions. My goal is to distill the basics into one website that is understandable and accesible. 

Here I have detailed the tools I've used and the process I went through to create genome assemblies, predict genes, and analyze results. When reading these walk-throughs, keep in mind that it's a rare day when everything works the first time. More time is spent troubleshooting installation problems and learning how to use a tool then is spent actually using the tool. So if you find yourself struggling to get things working, rest assured, that's a battle we all fight everyday. 





