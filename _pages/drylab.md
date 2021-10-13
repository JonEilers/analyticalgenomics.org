---
title: "Genome Bioinformatics"
permalink: /drylab/
author_profile: false
sidebar:
  nav: sidebar-main
classes: wide
entries_layout: grid

feature_row:
  - image_path: /assets/images/datacleaning.jpg
    title: "Data Cleaning"
    url: "/data-cleaning/"
  - image_path: /assets/images/genomeassembly.png
    title: "Genome Assembly"
    url: "/assembly/"
  - image_path: /assets/images/geneannotation.jpg
    title: "Genome Annotation"
    url: "/annotation/"

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Genomes contain life's code. Here are my studies of a sea cucumber's code"
---

{% include feature_row %}

## Please note that this section is currently being updated with more comprehensive content and as genome assembly and annotation takes awhile to run it may be some time. 

Here I have detailed the tools I've used and the process I went through to create the genome assembly, predict genes, and analyze results along with other projects I occasionally work on. Additionally, I will try to give an explanation of how these tools function and why it's important to understand how they work. When reading these posts, keep in mind that it's a rare day when everything works the first time. More time is spent troubleshooting installation problems and learning how to use a tool then is spent actually using the tool. So if you find yourself struggling to get things working, rest assured, that's a battle we all fight everyday. 

Genomes do not spontaneously create themselves. At least in silico. Once raw sequence data is acquired, quality and content is checked. Then an assembly tool is selected and run. When assembled, the assembly quality is assessed. If it's "good enough" repetitive element masking and gene prediction are performed. If everything checks out afterwards, the genome is ready for interrogation. Above are three pages containing content on the process for each of these steps. 

Working with large sequence datasets or on genomes requires some computational power. Most laptops or even desktop computers with their configurations maxed will not be able to handle the majority of genome bioinformatics. This is why most biology departments that are serious about incorporating bioinformatics into their program have access to a school computer cluster or purchase computational time on AWS. However, if you are freelance bioinformatician like me, neither of those are good options. My secret is that I realized servers have been around for awhile now and data centers replace them every so many years which means someone somewhere is surplusing them for cheap. For example [Newegg](https://www.newegg.com/p/pl?N=100852105%204016%20600031341) has a lot and they are quite cheap. Cheaper than buying a new shinny workstation or laptop. 

So I bought a barebones dell poweredge r910 for a few hundred and then wandered over to ebay where I bought 4 10 core cpus for ~$20, 16 600gb hard drives for about $10/hard drive, and ~362gb of ram for less then $1 per a 1g of ram. Let me tell you, this beast purrrrrs and also heats my apartment in the winter. For awhile the server was sitting on top of a dressor, but I upgraded to a rack eventually and also bought a refurbished UPS ebay to smooth out power flucuations. Total cost? probably sitting at close to $2k at this point, but I have a machine that can do genome bioinformatics for a fraction of the cost of what most people pay.  


