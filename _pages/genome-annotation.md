---
title: "Genome Annotation"
permalink: /genomeannotation/
toc: true
toc_label: "Genome Annotation"
author_profile: false
header:
  overlay_image: /assets/images/home/cuke1.jpg

---

<!--- add jbrowse, maybe some notes on how to evaluate gene annotations, transcriptome assembly tools (trinity), gemoma, bitacora, orthofinder -->

![image-left](/assets/images/resource_images/genome_size_vs_gene_count.png) Genome Annotation is a complex and difficult task. The average large genome contains anywhere from 20,000 to 30,000 genes and many duplicates. In addition to genes, genomes contain repetitive elements and non-coding DNA which play instrumental roles in genome organization and function. Finding and identifying each of these genomic elements is a huge undertaking requiring both automatic algorithmic processes and manual curation. After a program has found and identified these genomic features, there are numerous errors and an expert has to manually inspect elements of interest for accuracy before the annotation can be trusted. Below are some tools used in these process.

### Repeat Modeling and Masking

[RepeatMasker](http://repeatmasker.org/) is a program that screens DNA sequences for interspersed repeats and low complexity DNA sequences. The output of the program is a detailed annotation of the repeats that are present in the query sequence as well as a modified version of the query sequence in which all the annotated repeats have been masked.  

[RepeatModeler2](https://github.com/Dfam-consortium/RepeatModeler) is a de novo transposable element (TE) family identification and modeling package.   

### Gene Prediction 

[Braker2](http://exon.gatech.edu/braker1.html) is a pipeline for fully automated prediction of protein coding genes with GeneMark-ES/ET and AUGUSTUS in novel eukaryotic genomes    

[Maker2](http://www.yandell-lab.org/software/maker.html) is a portable and easily configurable genome annotation pipeline. 

[GenSAS](https://www.gensas.org/) is an online platform that provides a pipeline for whole genome structural and functional annotation. Users can upload genome sequences and select from a variety of tools for repeat masking, prediction of gene models and other structural features as well as functional annotation tools. 

### Targeted Gene Prediction
Currently, it is unwise to trust non-model organism gene models. Below are tools for identifying and generating gene models for genes of interestion in a genome assembly. 

[Bitacora](https://github.com/molevol-ub/bitacora) is a comprehensive tool for the identification and annotation of gene families in genome assemblies

[TGFam-Finder](https://github.com/tgfam-finder/scripts_ubuntu) is an annotation tool that works in the Linux OS environment for structural annotation of protein-coding genes containing target domains of interest. It also looks like a pain to set up. 

### Functional Annotation

[Team3-FunctionalAnnotation](https://github.com/compgenomics2019/Team3-FunctionalAnnotation) is a pipeline for functional annotation of prokaryote genomes. However, most of the tools present can also be used in eukaryotic genome functional annotation. 

[Interproscan](https://github.com/ebi-pf-team/interproscan): InterPro is a database which integrates together predictive information about proteins' function from a number of partner resources, giving an overview of the families that a protein belongs to and the domains and sites it contains.   

Users who have novel nucleotide or protein sequences that they wish to functionally characterise can use the software package InterProScan to run the scanning algorithms from the InterPro database in an integrated way.

[Infernal and rfam](https://github.com/Rfam/rfam-docs/blob/master/docs/source/genome-annotation.rst): The Rfam library of covariance models can be used to search sequences (including whole genomes) for homologues to known non-coding RNAs, in conjunction with the Infernal software.

[Diamond](https://github.com/bbuchfink/diamond) is a sequence aligner for protein and translated DNA searches, designed for high performance analysis of big sequence data. The key features are:
- Pairwise alignment of proteins and translated DNA at 500x-20,000x speed of BLAST.
- Frameshift alignments for long read analysis.
- Low resource requirements and suitable for running on standard desktops or laptops.
- Various output formats, including BLAST pairwise, tabular and XML, as well as taxonomic classification.

[tRNAscan](http://lowelab.ucsc.edu/tRNAscan-SE/) scans genome assemblies for tRNA.

### Genome Annotation Quality

[PATRIC](https://docs.patricbrc.org/tutorial/genome_quality_report/genome_quality_report.html) is a genome quality tool that looks at the functional roles present in an annotated genome to determine if the genome looks correct. Two separate mechanisms are used to predict the number of times each role should be found in the genome. A role is good if it occurs the predicted number of times; otherwise it is problematic.

[AEGeAn toolkit](https://aegean.readthedocs.io/en/stable/index.html) started as several distinct but related efforts to build tools for managing and analyzing whole-genome gene structure annotations. AEGeAn has brought these efforts together into a single library that includes executable programs as well as several data structures and modules callable via a C API. The AEGeAn Toolkit leverages a variety of parsers, data structures, and graphics capabilities available from the GenomeTools library 

[Apollo](https://genomearchitect.readthedocs.io/en/latest/) A collaborative, real-time, genome annotation web-based editor.

### Transcriptome alignment 

[HISAT2](http://ccb.jhu.edu/software/hisat2/manual.shtml#getting-started-with-hisat2) is a fast and sensitive alignment program for mapping next-generation sequencing reads.

[STAR](https://github.com/alexdobin/STAR) maps large sets of high-throughput sequencing reads to a reference genome with high levels of accuracy and speed.

[PASA](https://github.com/PASApipeline/PASApipeline/wiki), acronym for Program to Assemble Spliced Alignments (and pronounced 'pass-uh'), is a eukaryotic genome annotation tool that exploits spliced alignments of expressed transcript sequences to automatically model gene structures, and to maintain gene structure annotation consistent with the most recently available experimental sequence data. PASA also identifies and classifies all splicing variations supported by the transcript alignments. 

### Genome Feature File Manipulation Tools

[Liftoff](https://github.com/agshumate/Liftoff)s a tool that accurately maps annotations in GFF or GTF between assemblies of the same, or closely-related species.

[AGAT](https://github.com/NBISweden/AGAT/wiki#list-of-agat-tools-v021) is a suite of tools to handle gene annotations in any GTF/GFF format.
