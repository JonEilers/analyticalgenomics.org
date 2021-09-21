---
title: "Genome Assembly"
permalink: /assembly/
layout: collection
sidebar:
  nav: sidebar-main
classes: wide
entries_layout: grid

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Assemblying a genome is like putting together a million piece puzzle"
---
Genome Assembly is a [hard](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2005894). Even now with long read sequencing, [there is still a long ways to go](https://www.nature.com/articles/s41592-021-01057-y). In 1999, the human genome sequence was annouced. In 2021, a telomere to telomere genome assembly, with no gaps, was annouced. Even with the advances in technology and reduction in cost, many genomes cannot realistically be sequenced and assembled without a significant investment. For example, most salamander and newt species have genomes that are estimated to be 100 gigabase. [The largest genome sequenced so far was lungfish, with a genome size of 43 gigabases](https://www.nature.com/articles/s41586-021-03198-8?#Sec2). Lungfish have a diploid genome, which is fairly normal for a large animal. But plants and many other organisms are not diploid and require extra caution when assembling. One notable example is [wheat](https://www.sciencedirect.com/science/article/pii/S1672022920300590) which has chromosomes from [three different genomes and is an allohexaploid](https://www.nature.com/articles/s41586-020-2961-x0), meaning it has six sets of two chromsomes. Most genome assembler would not know what to do with this and would likely collapse the three sets in a psuedohaploid assembly, loosing a great deal of information in the process. 

Genome assembly approaches largely depend on the type and quality of data you have available to you. Sanger sequencing used to be the golden standard. Then illumina shotgun sequencing took over for awhile. Now we have long read sequencing using either PacBio or Oxford Nanopore Technologies platforms. Additional wetlab techniques such as [Hi-C](https://en.wikipedia.org/wiki/Chromosome_conformation_capture#Hi-C_(all-vs-all)) or [optical mapping](https://en.wikipedia.org/wiki/Optical_mapping) can further bring the assembly pieces together into chromosome length scaffolds. Any combination of the above can be found in most current genome assembly papers. However, if Illumina is all you have and genomes of closely related species are available, then reference assisted genome assebly is a possibility, but with caveats. 

<ul>
  {% for protocols in site.assembly %}
    <li>
      <h2><a href="{{ protocols.url }}">{{ protocols.title }}</a></h2>
       </li>
  {% endfor %}
</ul>

<!---
# Genome Assembly 
Many of the popular genome assemblers have the ability to do both short, long, and hybrid genome assembly. 

## Short read
Short read or next-gen sequencing is usually refering to the various Illumina sequencing types such as Hiseq and Miseq. All utiilize shotgun sequencing and produce highly accurate reads shorter than 1000 base pairs. 

## Long read

## Hybrid

## Reference Assisted

# Assembly Polishing and Analysis
-->
