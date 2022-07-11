---
title: "STAR: RNA-seq Mapping"
toc: true
toc_sticky: true
layout: single
permalink: /star/

excerpt: Mapping, both literal and metaphorical, has always been about our quest for knowing - Priyamvada Natarajan

gallery_multiqc:
  - url: /assets/images/annotation/star/hapog_long-reads.fasta_star_alignment_plot.png
    image_path: /assets/images/annotation/star/hapog_long-reads.fasta_star_alignment_plot.png
    title: hapog_long-reads
  - url: /assets/images/annotation/star/hapog_short-reads.fasta_star_alignment_plot.png
    image_path: /assets/images/annotation/star/hapog_short-reads.fasta_star_alignment_plot.png
    title: hapo-g short read
  - url: /assets/images/annotation/star/primary.genome.scf.fasta_star_alignment_plot.png
    image_path: /assets/images/annotation/star/primary.genome.scf.fasta_star_alignment_plot.png
    title: masurca assembly - high quality and unpolished
  - url: /assets/images/annotation/star/published_Ajap_genome.fasta_star_alignment_plot.png
    image_path: /assets/images/annotation/star/published_Ajap_genome.fasta_star_alignment_plot.png
    title: published apostichopus japonicus genome
  - url: /assets/images/annotation/star/scaffolds.ref.fa_star_alignment_plot.png
    image_path: /assets/images/annotation/star/scaffolds.ref.fa_star_alignment_plot.png
    title: masurca assembly - low quality and unpolished

---


To-do
- download one to two sra per a tissue type and development stage
- map to the five assemblies: the published, masurca high confidence, masurca low confidence, masurca polished short read, masurca polished long read
- detailed explanation of all aspects of star output and what it means 

# Introduction

Excerpt from the bbtools [website](https://jgi.doe.gov/data-and-tools/bbtools/)

BBTools is a suite of fast, multithreaded bioinformatics tools designed for analysis of DNA and RNA sequence data. BBTools can handle common sequencing file formats such as fastq, fasta, sam, scarf, fasta+qual, compressed or raw, with autodetection of quality encoding and interleaving. It is written in Java and works on any platform supporting Java, including Linux, MacOS, and Microsoft Windows and Linux; there are no dependencies other than Java (version 7 or higher). Program descriptions and options are shown when running the shell scripts with no parameters.

Excerpt from the STAR [publication](https://www.ncbi.nlm.nih.gov/pubmed/23104886)

To align our large (>80 billon reads) ENCODE Transcriptome RNA-seq dataset, we developed the Spliced Transcripts Alignment to a Reference (STAR) software based on a previously undescribed RNA-seq alignment algorithm that uses sequential maximum mappable seed search in uncompressed suffix arrays followed by seed clustering and stitching procedure. STAR outperforms other aligners by a factor of >50 in mapping speed, aligning to the human genome 550 million 2 × 76 bp paired-end reads per hour on a modest 12-core server, while at the same time improving alignment sensitivity and precision. In addition to unbiased de novo detection of canonical junctions, STAR can discover non-canonical splices and chimeric (fusion) transcripts, and is also capable of mapping full-length RNA sequences. Using Roche 454 sequencing of reverse transcription polymerase chain reaction amplicons, we experimentally validated 1960 novel intergenic splice junctions with an 80-90% success rate, corroborating the high precision of the STAR mapping strategy.

# Downloading Datasets

downloading rna-seq datasets for *Apostichopus japonicus*. Two datasets for each tissue type and for each developmental stage

SRA list:
SRR6192935 - blastula    
SRR6192939 - gastrula     
SRR6192940 - pre-auricularia     
SRR6192942 - mid-auricularia    
SRR6192946 - zygote    
SRR6192947 - cleavage    
SRR6192948 - pentactula    
SRR6192949 - juvenile    
SRR6192951 - post-auricularia    
SRR6192953 - doliolaria    
SRR6251590 - muscle
SRR6251597 - bodywall    
SRR6251614 - intestine    
SRR6251618 - respiratory tree    


```bash
# creating an environment, installing, and activating sratools by ncbi
conda create -n sratools sratoolkit
conda activate sratools

# temporarily downloading the sra and extracting the fastq files
fasterq-dump SRR6192935 SRR6192939 SRR6192940 SRR6192942 SRR6192946 SRR6192947 SRR6192948 SRR6192949 SRR6192951 SRR6192953 SRR6251590 SRR6251597 SRR6251614 SRR6251618 
```

# checking data quality using fastqc 

It's always a good idea to do a quick quality check on the raw data before using it. 
```bash
# creating an environment, installing, and activating fastqc

conda create -n fastqc fastqc
conda activate fastqc

# running fastqc, this will open a gui. Files can be opened from there
fastqc
```

Fastqc results for muscle and bodywall look weird are a little strange, but not worth trimming. Fastqc is showing some over represented sequences, that's not concerning to me as this is rna-seq and not genome data so I expect some over represented sequences. There is the usual 10 bases at the beginning of most reads that are highly variable, that's fine. I could trim that but recent publications have shown that aggressive trimming can mess with mapping results and differential gene expression analysis. So unless the data is particularly bad, I usually do not trim it. 

Some resources regarding this:
- [Micheal's  blog](https://www.michaelchimenti.com/2016/06/trim-rna-seq-reads/)   
- [basepairtech.com](https://www.basepairtech.com/blog/trimming-for-rna-seq-data/)    
- [Trimming of sequence reads alters RNA-Seq gene expression estimates](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4766705/)    
- [Read trimming is not required for mapping and quantification of RNA-seq reads at the gene level](https://academic.oup.com/nargab/article/2/3/lqaa068/5901066?login=true)

# mapping rna-seq data to genome assemblies

Thee are a number of rna-seq mapping tools available. The two popular ones that I am aware of are [STAR](https://academic.oup.com/bioinformatics/article/29/1/15/272537?login=true) and [HiSAT2](https://www.nature.com/articles/nmeth.3317?report=reader). For a more indepth look at what's available and the conveats see this paper titled [Comparison of Short-Read Sequence Aligners Indicates Strengths and Weaknesses for Biologists to Consider](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8087178/)

```bash
mamba create -n star star


conda create -n star star
conda activate star
```

generating genome indexes for the five genome assemblies to be check
```bash
for assembly in *
do

     mkdir ../genome_indices/$assembly

     STAR \
          --runThreadN 40 \
          --runMode genomeGenerate \
          --genomeDir /home/jon/Working_Files/japonicus_genome_project/star/genome_indices/$assembly \
          --genomeFastaFiles $assembly
done
```

running star
```bash

ulimit -n 100000

rna=('SRR6192935' 'SRR6192939' 'SRR6192940' 'SRR6192942' 'SRR6192946' 'SRR6192947' 'SRR6192948' 'SRR6192949' 'SRR6192951' 'SRR6192953' 'SRR6251590' 'SRR6251597' 'SRR6251614' 'SRR6251618')

for assembly in /home/jon/Working_Files/japonicus_genome_project/star/genome_indices/*
do
     base=$(basename "$assembly")
     echo making directory $base $'\n'
     mkdir $base

     for i in "${rna[@]}"
     do 
          
          echo making directory $base/$i/ $'\n'
          mkdir $base/$i/

          echo mapping "$i"_1 "$i"_2 $'\n'
        
          STAR \
               --runThreadN 40 \
               --genomeDir $assembly \
               --readFilesIn /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data/"$i"_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/rna-seq_data/"$i"_2.fastq \
               --outFileNamePrefix $base/"$i"/"$1"_ \
               --alignIntronMax 15000 \
               --twopassMode Basic \
               --outSAMtype BAM SortedByCoordinate 
     done
done
```

Note to self: I removed the alignmatemax and added twopassmode. It did slightly improve things. So I will have to come back to this at some point and change stuff. Maybe actually make some more graphs too or put it into pandas and analyze it. 

# Running multiqc

```bash
conda create -n multiqc multiqc
conda activate multiqc
```

running multiqc
```bash
multiqc --module star hapog_long-reads.fasta
multiqc --module star hapog_short-reads.fasta
multiqc --module star primary.genome.scf.fasta
multiqc --module star published_Ajap_genome.fasta
multiqc --module star scaffolds.ref.fa
```

I was hoping it would be able to compile all the results into one graph, but multiqc does not appear to be that sophisticated. It wouldn't difficult, just tidious, but this could easily be accomplished using excel pivot tables or pandas/R but that's more time than I want to put into this. So I just used multiqc on each assembly directory that I had star mapping results in. 

# Results

there was 14 rna-seq files that I mapped to each genome assembly. See below for the MultiQC graphs of those 14 rna-seq files on each assembly. 

{% include gallery id="gallery_multiqc" caption="STAR mapping results on five genome assemblies. a published one, two masurca, and two polished high quality masurca"  %}

Three things stand out to me.  Second is how many multi-mapping reads are in the low-quality masurca assembly. 

First is how bad the published assembly is relative to the other four. I know there are a lot of errors in the published assembly, but I expected that it would be better than when I mapped apostichopus japonicus reads to the apostichopus californicus genome assembly (not shown here). So that is very telling regarding how much that assembly should be trusted. 

Second is how many multi-mapping reads are in the low-quality masurca assembly. This isn't surprising considering the BUSCO results essentially suggested that. In that regard it is nice to have further confirmation.

hird is how little difference there is between the polished and unpolished high quality masurca assembly. Polishing doesn't generally change the assembly much, but it should improve mapping rates, or at the very least indel rates which star gives some stats on. See below. 


| UNIQUE READS:                                     | published ajap | Hapog-long-read | Hapog-short-read | masurca unpolished | low quality masurca  |
|---------------------------------------------------|----------------|-----------------|------------------|--------------------|----------------------|
|                    Uniquely mapped reads number | 10721935       | 11580798        | 11676918         | 11579053           | 10685194             |
|                         Uniquely mapped reads % | 62.65%         | 67.67%          | 68.24%           | 67.66%             | 62.44%               |
|                           Average mapped length | 193.46         | 194.46          | 194.41           | 194.46             | 194.68               |
|                        Number of splices: Total | 3748711        | 4043347         | 4059487          | 4042975            | 3621197              |
|             Number of splices: Annotated (sjdb) | 0              | 0               | 0                | 0                  | 0                    |
|                        Number of splices: GT/AG | 3681614        | 3991774         | 4007938          | 3991463            | 3575182              |
|                        Number of splices: GC/AG | 12103          | 12823           | 12993            | 12824              | 11377                |
|                        Number of splices: AT/AC | 4081           | 3810            | 3725             | 3810               | 3295                 |
|                Number of splices: Non-canonical | 50913          | 34940           | 34831            | 34878              | 31343                |
|                       Mismatch rate per base, % | 1.34%          | 1.37%           | 1.37%            | 1.37%              | 1.29%                |
|                          Deletion rate per base | 0.11%          | 0.11%           | 0.11%            | 0.11%              | 0.10%                |
|                         Deletion average length | 2.14           | 2.32            | 2.3              | 2.32               | 2.3                  |
|                         Insertion rate per base | 0.13%          | 0.08%           | 0.08%            | 0.08%              | 0.07%                |
|                        Insertion average length | 1.69           | 2.12            | 2.16             | 2.12               | 2.08                 |

For Illumina sequencing, presently the typical mismatch error rate are <0.5% and indel error rates <0.05%. 

avg lengh: 199
avg mapped length: 194

Most of the rRNA contain multiple highly sequence-similar paralogs, and hence RNA-seq reads will be mapped to multiple loci. Therefore, high percentage (>15%) of multi-mapping reads “% of reads mapped to multiple loci” is indicative of insufficient depletion of rRNA.

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4631051/