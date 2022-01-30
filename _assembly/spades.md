---
title: "Short read genome assembly using SPAdes"
permalink: /spades/
layout: single
type: assembly
header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Assembly By Short Sequences"

toc: true 

sidebar:
  nav: sidebar-main

---



## Installation

```bash
conda create -n spades
conda activate spades
conda install -c bioconda spades 
```

## Running SPAdes

```bash
spades.py \
    -1 /home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
    -2 /home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz \
    -t 70 \
    -m 450
    -o /home/jon/Working_Files/californicus_genome_project/spades
```