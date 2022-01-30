---
title: "Short read genome assembly using ABySS and SPAdes"
permalink: /short-read-assembly/
layout: single
type: assembly
header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Assembly By Short Sequences and St. Petersburg genome assembler "

toc: true 

sidebar:
  nav: sidebar-main

---

# Introduction
Short read assembly using Illumina short read data is an effective way to capture much of the "gene space" in a genome and is relatively inexpensive. As I noted previously, there is at trade off though, and that is there will be a significant number of long genes will be fragmented, you can't accurately determine the number of duplicated genes, and it will be impossible to meaningfully investigate gene regulation. But if you just want genes, then by all means order some Illumina sequencing for your species of choice. There are loads of interesting questions about genes that are worth studying. 

## Data

Data used in the assemblies below can be found at [here](https://www.ncbi.nlm.nih.gov/sra?linkname=bioproject_sra_all&from_uid=720913)

```bash
# creating a conda environment for the ncbi sra tools
conda create -n sratools
conda install -c bioconda sra-tools conda activate sratools
conda activate sratools

# command to dowload the sra file
prefetch SRR14197331

# command to extract the fastq file
cd SRR14197331
fasterq-dump *.sra
```

Interleaving the paired end data using [Seqtk](https://github.com/lh3/seqtk) to make abyss happy when assemblying. Seqtk is a simple and powerful tool for quickly manipulating fastq/a files  and was created by [Heng Li](https://github.com/lh3) who developed Minimap and bwa. It's not to be confused with the equally power tool [Seqkit](https://github.com/shenwei356/seqkit) which is more versatile and better maintained. 

```bash
conda create -n seqtk
conda activate seqtk
conda install -c bioconda seqtk

# interleaving the paired end data
seqtk mergepe \
    P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
    P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz \
    > pcali.merged.fq

# getting rid of any non-paired reads
seqtk dropse pcali.merged.fq > pcali.merged.dropse.fq
```

For kicks and giggles here is how to do it using seqkit
```bash
conda create -n seqkit
conda activate seqkit
conda install -c bioconda seqkit 

# note this will drop any non-paired reads. It also creates two separate files
seqkit pair \
    -1 P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
    -2 P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz \
    -o pcali.merged.fa.gz \
    -j 70
```

seqtk is apparently significantly faster than seqkit or maybe seqkit is being slow because I have it compressing the fastq files whereas I didn't compress the output for seqtk. 

## ABySS

[ABySS](https://genome.cshlp.org/content/27/5/768) was first released in [2009](https://genome.cshlp.org/content/19/6/1117) and has been actively maintained and developed. In their own words ABySS is "intended for short paired-end reads and genomes of all sizes." See below for an example of assembling the genome of *Apostichopus californicus*. 

### Installation

Creating a conda environment and installing abyss along with optional dependencies suggest at the [abyss github repository](https://github.com/bcgsc/abyss#citation)

```bash
conda create -n abyss
conda activate abyss
conda install -c bioconda abyss

# optional dependencies
conda install -c bioconda samtools
conda install -c conda-forge pigz zsh
```

A quick glance at the FastQC analysis reveals high quality reads with very little in the way of adapter contamination. These two assemblers do recommend trimming the reads, but the data is already [quite clean](/short_read_quality/#results) so I will actually skip that step and move straight to genome assembly. 

### Running abyss

Running abyss on the illumina short read data. the kmer value of 111 was determine previously using during the data quality control step using [kmergenie](/kmergenie/), [kat](/kat/), and [jellyfish/genomescope](/genomescope/). 'j' is the number cpu threads and 'B' is the bloom filter memory budget which can be extrapolated from the guidelines described on github. 

```bash
abyss-pe \
    name=pcali \
    k=111 \
    B=18G \
    j=70 \
    -C /home/jon/Working_Files/californicus_genome_project/abyss \
    in='/home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/pcali.merged.dropse.fq'
```

## SPAdes

While not as old as ABySS, [SPAdes](https://www.liebertpub.com/doi/abs/10.1089/cmb.2012.0021) has certainly been around awhile and is also actively maintained and developed as evidenced by the plethora of use cases found on their [github respository](https://github.com/ablab/spades) and described in their latest [paper](https://currentprotocols.onlinelibrary.wiley.com/doi/abs/10.1002/cpbi.102). While initially intended for bacterial genome assembly, its use cases have expanded to include assemblying Eukaryotic genomes so I have included it here as an example. 

### Installation

Creating a conda environment and installing [SPAdes](https://github.com/ablab/spades)
```bash
conda create -n spades
conda activate spades
conda install -c bioconda spades 
```

### Running SPAdes

SPAdes automatically determines the ideal Kmer value to be used for assembly. So for a basic run the only parameteres needed are the number cpu threads and max amount of memory. Although the default kmer range is 21, 33, 55, 77, which is not the 110ish that was estimated using kmergenie. So there is the possibility that increasing kmer size would improve the assembly. However, in this instance I will just use the default. 

```bash
spades.py \
    -1 /home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
    -2 /home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz \
    -t 70 \
    -m 400 \
    -o /home/jon/Working_Files/californicus_genome_project/spades
```

There is some discussion on what parameters are best for assemblying eukaryote genomes. Github [Issues](https://github.com/ablab/spades/issues/708) can be a useful source of information for finding ways to improve the genome assembly or if there might be problems with your data. Out of curiousity I reran spades using the --isolate option

```bash
spades.py \
    -1 /home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz \
    -2 /home/jon/Working_Files/sea_cuke_species_data/pcali_backup_data/raw_data/P_cali_g/P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz \
    --isolate \
    -t 70 \
    -m 400 \
    -o /home/jon/Working_Files/californicus_genome_project/spades
```


## Results 