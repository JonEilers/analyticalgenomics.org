---
title: "Quast-lg: Platanus-Allee Assembly Analysis"
toc: true
toc_sticky: true
layout: single

gallery:
  - url: /assets/images/quast/platanus-allee/circos.png
    image_path: /assets/images/quast/platanus-allee/circos.png
    title: "Circos plot of A. japonicus genome vs P. californicus"
    caption: 
  - url: /assets/images/quast/platanus-allee/GC_content_plot.jpg
    image_path: /assets/images/quast/platanus-allee/GC_content_plot.jpg
    title: "Cumulative read length plot"
    caption: 
  - url: /assets/images/quast/platanus-allee/misassemblies_plot.jpg
    image_path: /assets/images/quast/platanus-allee/misassemblies_plot.jpg
    title: "genome misassemblies"
    caption: 
  - url: /assets/images/quast/platanus-allee/NGx_plot.jpg
    image_path: /assets/images/quast/platanus-allee/NGx_plot.jpg
    title: 
    caption:
  - url: /assets/images/quast/platanus-allee/Nx_plot.jpg
    image_path: /assets/images/quast/platanus-allee/Nx_plot.jpg
    title: 
    caption: 
  - url: /assets/images/quast/platanus-allee/out_consensusScaffold_GC_content_plot.jpg
    image_path: /assets/images/quast/platanus-allee/out_consensusScaffold_GC_content_plot.jpg
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
/home/jon/anaconda3/envs/biotools/bin/quast --large --eukaryote -r A_jap_genome.fasta -g A_jap-annotation.gff3 -t 70 --circos --gene-finding --conserved-genes-finding --operon A_jap-annotation.gff3 Patanus-Allee/Platanus_allee_v2.0.2/out_consensusScaffold.fa
```

### Results  

{% include gallery %}


#### Busco Analysis

Below are some of the results from the intial analysis. For a better formated and more details view the report [here](/assets/images/quast/platanus-allee/report.html)

```
  C:80.2%[S:75.6%,D:4.6%],F:12.9%,M:6.9%,n:303

  243 Complete BUSCOs (C)
  229 Complete and single-copy BUSCOs (S)
  14  Complete and duplicated BUSCOs (D)
  39  Fragmented BUSCOs (F)
  21  Missing BUSCOs (M)
  303 Total BUSCO groups searched
```

#### Assembly Metrics


Assembly length and rough statistics

| Assembly                   | out_consensusScaffold | Assembly         | out_consensusScaffold | 
|----------------------------|-----------------------|------------------|-----------------------| 
| # contigs (>= 0 bp)        | 684753                | Largest contig   | 134774                | 
| # contigs (>= 1000 bp)     | 119515                | Total length     | 615500166             | 
| # contigs (>= 5000 bp)     | 40704                 | Reference length | 804620767             | 
| # contigs (>= 10000 bp)    | 19313                 | GC (%)           | 37.28                 | 
| # contigs (>= 25000 bp)    | 4079                  | Reference GC (%) | 37.01                 | 
| # contigs (>= 50000 bp)    | 499                   | N50              | 13429                 | 
| Total length (>= 0 bp)     | 838807926             | NG50             | 9218                  | 
| Total length (>= 1000 bp)  | 718140464             | N75              | 7121                  | 
| Total length (>= 5000 bp)  | 533142572             | NG75             | 3257                  | 
| Total length (>= 10000 bp) | 382544208             | L50              | 12841                 | 
| Total length (>= 25000 bp) | 150565797             | LG50             | 21371                 | 
| Total length (>= 50000 bp) | 31785947              | L75              | 28700                 | 
| # contigs                  | 61935                 | LG75             | 58084                 | 



Assembly characteristics

| Assembly                    | out_consensusScaffold | Assembly                 | out_consensusScaffold | 
|-----------------------------|-----------------------|--------------------------|-----------------------| 
| # misassemblies             | 358                   | Genome fraction (%)      | 2.925                 | 
| # misassembled contigs      | 294                   | Duplication ratio        | 1.327                 | 
| Misassembled contigs length | 1675637               | # N's per 100 kbp        | 105.49                | 
| # local misassemblies       | 151                   | # mismatches per 100 kbp | 3685.76               | 
| # scaffold gap ext. mis.    | 0                     | # indels per 100 kbp     | 574.97                | 
| # scaffold gap loc. mis.    | 29                    | # genomic features       | 0 + 0 part            | 
| # possible TEs              | 2                     | Complete BUSCO (%)       | 80.2                  | 
| # unaligned mis. contigs    | 5241                  | Partial BUSCO (%)        | 12.87                 | 
| # unaligned contigs         | 42897 + 18869 part    | Largest alignment        | 19308                 | 
| Unaligned length            | 584492667             | Total aligned length     | 30343884              | 



