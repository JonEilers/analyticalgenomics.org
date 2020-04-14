---
title: "Quast-lg: MaSuRCA Assembly Analysis"
toc: true
toc_sticky: true
layout: single

gallery:
  - url: /assets/images/quast/masurca/circos.png
    image_path: /assets/images/quast/masurca/circos.png
    title: "Circos plot of A. japonicus genome vs P. californicus"
    caption: 
  - url: /assets/images/quast/masurca/cumulative_plot.jpg
    image_path: /assets/images/quast/masurca/cumulative_plot.jpg
    title: "Cumulative read length plot"
    caption: 
  - url: /assets/images/quast/masurca/GC_content_plot.jpg
    image_path: /assets/images/quast/masurca/GC_content_plot.jpg
    title: "genome misassemblies"
    caption: 
  - url: /assets/images/quast/masurca/NGx_plot.png
    image_path: /assets/images/quast/masurca/NGx_plot.png
    title: 
    caption:
  - url: /assets/images/quast/masurca/Nx_plot.jpg
    image_path: /assets/images/quast/masurca/Nx_plot.jpg
    title: 
    caption: 
  - url: /assets/images/quast/masurca/masurca_p_cali_GC_content_plot.jpg
    image_path: /assets/images/quast/masurca/masurca_p_cali_GC_content_plot.jpg
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
  OS: Linux-5.0.0-36-generic-x86_64-with-debian-buster-sid (linux_64)
  Python version: 3.6.7
  CPUs number: 80
```

### Command   
```
/home/jon/anaconda3/envs/biotools/bin/quast --large --eukaryote -r A_jap_genome.fasta -g A_jap-annotation.gff3 -m 900 -t 70 -k --circos -f -b -1 P_cali_g_1.fq -2 P_cali_g_2.fq masurca_p_cali.fasta
```

### Results  

{% include gallery %}


#### Busco Analysis

Below are some of the results from the intial analysis. For a better formated and more details view the report [here](/assets/images/quast/masurca/report.html)

```
  C:73.6%[S:57.4%,D:16.2%],F:15.8%,M:10.6%,n:303

  223 Complete BUSCOs (C)
  174 Complete and single-copy BUSCOs (S)
  49  Complete and duplicated BUSCOs (D)
  48  Fragmented BUSCOs (F)
  32  Missing BUSCOs (M)
  303 Total BUSCO groups searched
```

#### Assembly Metrics


Assembly length and rough statistics

| Assembly                   | masurca_p_cali | Assembly         | masurca_p_cali | 
|----------------------------|----------------|------------------|----------------| 
| # contigs (>= 0 bp)        | 191698         | Largest contig   | 86428          | 
| # contigs (>= 1000 bp)     | 132041         | Total length     | 767742047      | 
| # contigs (>= 5000 bp)     | 52526          | Reference length | 804620767      | 
| # contigs (>= 10000 bp)    | 20629          | GC (%)           | 37.41          | 
| # contigs (>= 25000 bp)    | 1896           | Reference GC (%) | 37.01          | 
| # contigs (>= 50000 bp)    | 63             | N50              | 8766           | 
| Total length (>= 0 bp)     | 795194727      | NG50             | 8320           | 
| Total length (>= 1000 bp)  | 763487445      | N75              | 4720           | 
| Total length (>= 5000 bp)  | 560909136      | NG75             | 4200           | 
| Total length (>= 10000 bp) | 334751931      | L50              | 25877          | 
| Total length (>= 25000 bp) | 61822684       | LG50             | 28037          | 
| Total length (>= 50000 bp) | 3718801        | L75              | 55593          | 
| # contigs                  | 136526         | LG75             | 61803          | 


Assembly characteristics

| Assembly                    | masurca_p_cali      | Assembly                 | masurca_p_cali | 
|-----------------------------|---------------------|--------------------------|----------------| 
| # misassemblies             | 413                 | Duplication ratio        | 1.462          | 
| # misassembled contigs      | 367                 | # N's per 100 kbp        | 139.85         | 
| Misassembled contigs length | 1579758             | # mismatches per 100 kbp | 3849.18        | 
| # local misassemblies       | 230                 | # indels per 100 kbp     | 619.19         | 
| # scaffold gap ext. mis.    | 0                   | # genomic features       | 0 + 0 part     | 
| # scaffold gap loc. mis.    | 41                  | Complete BUSCO (%)       | 73.6           | 
| # possible TEs              | 8                   | Partial BUSCO (%)        | 15.84          | 
| # unaligned mis. contigs    | 4309                | Largest alignment        | 18961          | 
| # unaligned contigs         | 110027 + 24449 part | Total aligned length     | 32630849       | 
| Unaligned length            | 733479175           | NGA50                    | -              | 
| Genome fraction (%)         | 2.933               | K-mer-based compl. (%)   | 0.44           | 



