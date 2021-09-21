---
title: "Adapters and FastQC report"
layout: single
permalink: /fastqc/
---
## Purpose: Check md5, create adapter file, and get summary data on sequences 

I need to add content about how to clean data if it is messy. So some adapter trimming tools, filtering tools, etc. I also need to add a section on nanopore data cleaning to this. 

### Adapters and Index used for sequencing
``` 
5' Adapter
5'-AATGATACGGCGACCACCGAGATCTACACTCTTTCCCTACACGACGCTCTTCCGATCT-3' 
3' Adapter - the index is ATCACG 
5'-GATCGGAAGAGCACACGTCTGAACTCCAGTCACATCACGATCTCGTATGCCGTCTTCTGCTTG-3' 
```


### FastQC run to check sequence data
```
fastqc -o QC --noextract -t 5 -a adapters.txt P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz 
```

#### Results
Results can be found [here](/fastqc1/) and [here](/fastqc2/)

#### Novogene report

[Report](/data-cleaning/Novogene/)


### Resources 
An good walk through of the different fastqc outputs and their meaning in addition to quality control processing: [FastQC info by open repository](https://openrepository.aut.ac.nz/bitstream/handle/10292/5170/FASTQC%20analysis%20guide.pdf?sequence=5&isAllowed=y)


Excellent comparisons of bad versus good quality sequence data and the graphs fastqc produces: [Fastqc tutorial and faq by MSU](https://rtsf.natsci.msu.edu/genomics/tech-notes/fastqc-tutorial-and-faq/)


Also contains excellent comparisons of bad versus good quality sequence data along with some processing steps: [Bioinformatics Core Training](https://bioinformatics-core-shared-training.github.io/cruk-autumn-school-2017/Introduction/SS_DB/Materials/Lectures/Lecture2_qualityControl_artefactRemoval_DB.pdf)
