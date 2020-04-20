---
title: "GenSAS"
toc: true
toc_sticky: true
layout: single

---

## GenSAS
Genome annotation server

### Background

Excerpt from the GenSAS [website](https://www.gensas.org/)

The Genome Sequence Annotation Server (GenSAS) is an online platform that provides a pipeline for whole genome structural and functional annotation for eukaryotes and prokaryotes. Users can upload genome sequences and select from a variety of tools for repeat masking, prediction of gene models and other structural features as well as functional annotation tools.  GenSAS integrates with JBrowse and Apollo to provide visualization and editing.

### Process

GenSAS is an online server. It requires creating an account. After that the genome assembly and any other relevant data is uploaded. I have described the process I went through below.

I uploaded the mapped RNA-seq data in a BAM file and the soft-masked and 1kbp filtered redundans genome assembly. I ran the below software that is available on the server. Braker produced the best gene predictions and that is what was ultimate used as the gene model set for the functional annotation step as seen below. The final annotation set includes the 

Genes and Other Predictions
* BLAT
* BRAKER
* DIAMOND proteins
* EvidenceModeler
* GeneMarkES
* getorf
* HISAT2
* PASA
* RNAmmer
* SNAP
* SSR Finder
* tRNAcan-SE

Functional Annotation
* DIAMOND refseq invert
* DIAMOND swissprot
* DIAMOND trembl
* InterProScan
* Pfam
* SignalP

Final Merged Annotated Genome
* Annotations a1