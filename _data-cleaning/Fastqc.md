---
title: "Adapters and FastQC report"
layout: single
permalink: /fastqc/
toc: true 
toc_sticky: true

excerpt: ""
---
## Introduction
You have your data, you've checked the file integrity and are confident no data was corrupted during the transfer process. It's now time to take a quick look to see what you'll be working with. Things such as average read length, duplications, base and sequence quality, GC content, sequence length distribution, overrepresented sequences, adapter contamination, the list goes on. Thankfully, someone created a really good tool for checking all those things. It's sort of a one stop shop for read quality analysis and it's called [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). 

## Installing Fastqc
You can download it from the website, or you can go use the package manager called [conda](https://docs.conda.io/en/latest/miniconda.html) to install it. 

Installing using conda
```bash
# create a conda environment. This helps to keep software dependencies isolated from eachother when using a lot of different bioinformatic tools. 
conda create -n fastqc

# installing fastqc
conda install -c bioconda fastqc 
```

## Running fastQC
Fastqc can be used via command line or via its GUI

```bash
# activating the fastqc conda environment
conda activate fastqc

# running fastqc from the command line
fastqc \
    -o QC \
    --noextract \
    -t 5 \
    -a adapters.txt \
    P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
    P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz 

# This will open the GUI
fastqc
```

## Results
Results can be found [here](/fastqc1/) and [here](/fastqc2/)
The quality of the data looks pretty darn good. There might be some PCR amplication represented by the repeating 'G'. The beginning of the reads could be smoother, but nothing to write home about. In fact, I would say this data doesn't need any read filtering or trimming.

## Novogene report
Novogene also provided a quality report which agrees with what FastQC shows. Here is the [Report](/data-cleaning/Novogene/)


## Resources 
I could go into details about what each of the FastQC graphs mean and how to read them, but others have done a great job. Here is a a good walk through of the different fastqc outputs and their meaning in addition to quality control processing: [FastQC info by open repository](https://openrepository.aut.ac.nz/bitstream/handle/10292/5170/FASTQC%20analysis%20guide.pdf?sequence=5&isAllowed=y)


Excellent comparisons of bad versus good quality sequence data and the graphs fastqc produces: [Fastqc tutorial and faq by MSU](https://rtsf.natsci.msu.edu/genomics/tech-notes/fastqc-tutorial-and-faq/)


Also contains excellent comparisons of bad versus good quality sequence data along with some processing steps: [Bioinformatics Core Training](https://bioinformatics-core-shared-training.github.io/cruk-autumn-school-2017/Introduction/SS_DB/Materials/Lectures/Lecture2_qualityControl_artefactRemoval_DB.pdf)
