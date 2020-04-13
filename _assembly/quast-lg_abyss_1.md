---
title: "Quast-lg: Abyss-pe Assembly Analysis"
toc: true
toc_sticky: true
layout: single

gallery:
  - url: /assets/images/quast/circos.png
    image_path: /assets/images/quast/circos.png
    title: "Circos plot of A. japonicus genome vs P. californicus"
    caption: 
  - url: /assets/images/quast/cumulative_plot.jpg
    image_path: /assets/images/quast/cumulative_plot.jpg
    title: "Cumulative read length plot"
    caption: 
  - url: /assets/images/quast/misassemblies_plot.jpg
    image_path: /assets/images/quast/misassemblies_plot.jpg
    title: "genome misassemblies"
    caption: 
  - url: /assets/images/quast/NGx_plot.jpg
    image_path: /assets/images/quast/NGx_plot.jpg
    title: 
    caption:
  - url: /assets/images/quast/Nx_plot.jpg
    image_path: /assets/images/quast/Nx_plot.jpg
    title: 
    caption: 
  - url: /assets/images/quast/P_cali_8_GC_content_plot.jpg
    image_path: /assets/images/quast/P_cali_8_GC_content_plot.jpg
    title: "Read GC content"
    caption:        
---

## QUAST-LG 
Quality analysis of genome assembly

### Background
Excerpt from the QUAST-LG [manual](http://cab.spbu.ru/files/quast/latest-docs/manual.html)

QUAST stands for QUality ASsessment Tool. The tool evaluates genome assemblies by computing various metrics. This document provides instructions for the general QUAST tool for genome assemblies, MetaQUAST, the extension for metagenomic datasets, QUAST-LG, the extension for large genomes (e.g., mammalians), and Icarus, the interactive visualizer for these tools.

Their website can be found [here](http://cab.spbu.ru/software/quast-lg/)

### Installation

Quast was installed using conda. 

```
conda install -c bioconda quast 
```

Installation information
```
Version: 5.0.2

System information:
  OS: Linux-4.15.0-52-generic-x86_64-with-debian-buster-sid (linux_64)
  Python version: 2.7.16
  CPUs number: 20
```

### Command   
```
/home/jeilers/anaconda2/bin/quast --large --eukaryote --k-mer-stats --circos --gene-finding --threads 10 --conserved-genes-finding /media/jeilers/Elements/P_cali_genome/Working Files/abyss-pe/P_cali-8.fa -r /media/jeilers/Elements/P_cali_genome/Working Files/A_jap_genome.fasta -g /media/jeilers/Elements/P_cali_genome/Working Files/A_jap-annotation.gff3 -o ../../media/jeilers/Elements/P_cali_genome/
```

### Results  

{% include gallery %}


#### Busco Analysis

Below are some of the results from the intial analysis. For a better formated and more details view the report [here](/Research_Documents/Quast-Results/report.html)

|Number of genes|Percent off total|Description|
|124|41.0%|	Complete BUSCOs (C)|
|89|29.4%|	Complete and single-copy BUSCOs (S)|
|35|11.6%|	Complete and duplicated BUSCOs (D)|
|89|29.4%|	Fragmented BUSCOs (F)|
|90|29.6%|	Missing BUSCOs (M)|
|303||	Total BUSCO groups searched|

#### Assembly Metrics


Assembly length and rough statistics

| Assembly         | P_cali_8  | Assembly                   | P_cali_8   | 
|------------------|-----------|----------------------------|------------| 
| # contigs        | 74045     | # contigs (>= 0 bp)        | 1851769    | 
| Largest contig   | 103765    | # contigs (>= 1000 bp)     | 131899     | 
| Total length     | 468431423 | # contigs (>= 5000 bp)     | 39026      | 
| Reference length | 804620767 | # contigs (>= 10000 bp)    | 8852       | 
| GC (%)           | 36.89     | # contigs (>= 25000 bp)    | 328        | 
| Reference GC (%) | 37.01     | # contigs (>= 50000 bp)    | 11         | 
| N50              | 6822      | Total length (>= 0 bp)     | 1007051673 | 
| NG50             | 3937      | Total length (>= 1000 bp)  | 584516010  | 
| N75              | 4694      | Total length (>= 5000 bp)  | 332013941  | 
| L50              | 22207     | Total length (>= 10000 bp) | 125745248  | 
| LG50             | 54864     | Total length (>= 25000 bp) | 10357409   | 
| L75              | 43012     | Total length (>= 50000 bp) | 656503     | 



Assembly characteristics

| Assembly                    | P_cali_8           | Assembly                 | P_cali_8  | 
|-----------------------------|--------------------|--------------------------|-----------| 
| # misassemblies             | 41                 | Unaligned length         | 451104938 | 
| # misassembled contigs      | 39                 | Genome fraction (%)      | 1.638     | 
| Misassembled contigs length | 266331             | Duplication ratio        | 1.324     | 
| # local misassemblies       | 191                | # N's per 100 kbp        | 208.89    | 
| # scaffold gap ext. mis.    | 0                  | # mismatches per 100 kbp | 3762.96   | 
| # scaffold gap loc. mis.    | 17                 | # indels per 100 kbp     | 506.62    | 
| # possible TEs              | 4                  | Largest alignment        | 14876     | 
| # unaligned mis. contigs    | 2173               | Total aligned length     | 16328463  | 
| # unaligned contigs         | 61729 + 12262 part | K-mer-based compl. (%)   | 0.29      | 


### Thoughts
* haploid genome size is predicted to be ~468 mb or diploid at ~936 mb
* 41% of conserved buscos are present, this is actually rather low. Including partial busco, it is only a little under 70% of buscos. 


