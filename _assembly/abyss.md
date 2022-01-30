---
title: "Short read genome assembly using ABySS"
permalink: /abyss/
layout: single
type: assembly
header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "St. Petersburg genome assembler"

toc: true 

sidebar:
  nav: sidebar-main

---

## Installation

```bash
conda create -n abyss
conda activate abyss
conda install -c bioconda abyss

# option dependencies
conda install -c bioconda samtools
conda install -c conda-forge pigz zsh
```

## Running abyss

```bash
abyss-pe \
    name=pcali \
    k=111 \
    B=18G \
    j=70 \
    -C /home/jon/Working_Files/californicus_genome_project/abyss \
    in='/home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
    /home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz'
```