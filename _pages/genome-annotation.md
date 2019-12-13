---
title: "Genome Annotation"
permalink: /genomeannotation/
toc: true
toc_label: "Genome Annotation"
---

![image-left](/assets/images/resource_images/genome-annotation.gif){: .align-right width="500px" height="500px"} Genome Annotation is a complex and difficult task. The average large genome contains anywhere from 20,000 to 30,000 genes and many duplicates. In addition to genes, genomes contain repetitive elements and non-coding DNA which play instrumental roles in genome organization and function. Finding and identifying each of these genomic elements is a huge undertaking requiring both automatic algorithmic processes and manual curation. After a program has found and identified these genomic features, there are numerous errors and an expert has to manually inspect elements of interest for accuracy before the annotation can be trusted. Below are some tools used in these process.

### Repeat Annotating and Masking

[RepeatMasker](http://repeatmasker.org/) is a program that screens DNA sequences for interspersed repeats and low complexity DNA sequences. The output of the program is a detailed annotation of the repeats that are present in the query sequence as well as a modified version of the query sequence in which all the annotated repeats have been masked.  

[RepeatModeler2](https://github.com/Dfam-consortium/RepeatModeler) is a de novo transposable element (TE) family identification and modeling package.  

[Repbase](https://www.girinst.org/repbase/update/index.html) is a database of prototypic sequences representing repetitive DNA from different eukaryotic species. Repbase is being used in genome sequencing projects worldwide as a reference collection for masking and annotation of repetitive DNA (e.g. by RepeatMasker or CENSOR).  

### Gene Annotation 

[Augustus](http://bioinf.uni-greifswald.de/augustus/) is a program that predicts genes in eukaryotic genomic sequences.  

[Braker2](http://exon.gatech.edu/braker1.html) is a pipeline for fully automated prediction of protein coding genes with GeneMark-ES/ET and AUGUSTUS in novel eukaryotic genomes   

[Genemark](http://exon.gatech.edu/GeneMark/) is a family of gene prediction programs.  

[Maker2](http://www.yandell-lab.org/software/maker.html) is a portable and easily configurable genome annotation pipeline.  

### Genome Quality

[PATRIC](https://docs.patricbrc.org/tutorial/genome_quality_report/genome_quality_report.html) is a genome quality tool that looks at the functional roles present in an annotated genome to determine if the genome looks correct. Two separate mechanisms are used to predict the number of times each role should be found in the genome. A role is good if it occurs the predicted number of times; otherwise it is problematic.

### Transcriptome alignment 

[HISAT2](http://ccb.jhu.edu/software/hisat2/manual.shtml#getting-started-with-hisat2) is a fast and sensitive alignment program for mapping next-generation sequencing reads (whole-genome, transcriptome, and exome sequencing data) against the general human population (as well as against a single reference genome).

[STAR](https://github.com/alexdobin/STAR) maps large sets of high-throughput sequencing reads to a reference genome with high levels of accuracy and speed.

### Gene Family Clustering

[OrthoMCL](https://orthomcl.org/orthomcl/)is a genome-scale algorithm for grouping orthologous protein sequences. It provides not only groups shared by two or more species/genomes, but also groups representing species-specific gene expansion families. So it serves as an important utility for automated eukaryotic genome annotation.

[COG](http://archive-dtd.ncbi.nlm.nih.gov/COG/) is another algorithm and database for clustering genes.
