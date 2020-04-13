---
title: "KAT: Kmer Analysis of Raw Sequence Data"

toc: true
toc_sticky: true
author_profile: false
layout: single

gallery:
  - url: /assets/images/kat/kat-gcp.mx.png
    image_path: /assets/images/kat/kat-gcp.mx.png
    title: "Comparison of GC content and K-mer coverage from the input"
    caption: "Comparison of GC content and K-mer coverage from the input"
  - url: /assets/images/kat/P_cali_katcomp-main.mx.density.png
    image_path: /assets/images/kat/P_cali_katcomp-main.mx.density.png
    title: "Model curve placed over data showing how well it fits the kmer distribution"
    caption: "Model curve placed over data showing how well it fits the kmer distribution"
  - url: /assets/images/kat/P_cali_kat_spectra.png
    image_path: /assets/images/kat/P_cali_kat_spectra.png
    title: "kmer distribution in data. Predicted Best kmer value denoted by red dashed line"
    caption: "Linear GenomeScope Profile"    
---

## KAT
Kmer Analysis of Raw Sequence Data

### Background
The below exerpt is from the KAT [website](https://kat.readthedocs.io/en/latest/)   

KAT provides a suite of tools that, through the use of k-mer counts, help the user address or identify issues such as determining sequencing completeness for assembly, assessing sequencing bias, identifying contaminants, validating genomic assemblies and filtering content. KAT is geared primarily to work with high-coverage genomic reads from Illumina devices, although can work with any fasta or fastq sequence file.

At it’s core KAT exploits the concept of k-mer spectra (histograms plotting number of distinct k-mers at each frequency). By studying properties of the k-mer spectra it’s possible to discover important information about the data quality (level of errors, sequencing biases, completeness of sequencing coverage and potential contamination) and genomic complexity (size, karyotype, levels of heterozygosity and repeat content). Further information can be gleaned through pairwise comparison of spectra, making KAT useful for WGS library comparisons and assembly validation.

### Installion information

KAT was installed using conda. Due to dependency conflicts, KAT required creating a separate conda environment prior to installation. See below for KAT package details
```
kat                       2.4.2            py36he1b856b_0    bioconda
```
To install
```
conda create -name kat
conda activate kat
conda install -c bioconda kat 
```

### Commands

KAT commands
```
kat comp -t 70 -n -o P_cali_katcomp ../P_cali_g_1.fq  ../P_cali_g_2.fq
kat plot spectra-mx -i -x 200 -o P_cali_kat_spectra.png P_cali_katcomp-main.mx
kat gcp -t 70 -m 32 ../P_cali_g_1.fq  ../P_cali_g_2.fq
```

### Figures
{% include gallery %}