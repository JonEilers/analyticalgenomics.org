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
 
# Introduction

Gene expression data is critical to creating high quality and high confidence gene models. Reads mapped to genome assembly provide evidence of where genes can be found and the structure of the gene such as the splices sites of introns and exons. Without it, gene model prediction can only be accomplished using homology-based methods and looking for open readings frames and cannonical splice sites. Homology-based methods are sensitive to differences in the genes. Meaning that this will only work well on conserved sequences and will miss recently evolved genes and mutations that significantly impact the structure and sequence of the gene. Predicting genes using just open reading frames and cannonical splice sites is even more fraught because start and stop codons appear frequently throughtout the genome without being associated with any gene. Using open reading frames only will result in a large number of spurious genes. 

Mapping rna-seq data to a genome assembly is also a useful way to evaulate the genome assembly. For example, STAR outputs a summary file contains various statistics related to the mapping rates. This includes stats on deletions/insertions which can be suggestive of a low quality assembly, high heterozygousity, or low quality reads. Seeing high deltion/insertions stats is a good reason to investigate which of those is true. Additionally, uniquely mapping reads is also an important number as high quality assembly should achieve a high unique mapping rate, but this will also be species specific. However, if the percentage of reads mapping to multiple places is higher than or close to the same as the unique mapping rate then there is reason to be suspicious of the genome assembly. 

In recent years Oxford Nanopore Technologies and Pacbio have both released long read rna sequencing. In the case of Oxford Nanopore Technologies, the platform directly sequences the rna without needing any conversion to DNA or amplification. Meaning you can potentially get an accurate picture of the rna modifications and expression levels. Unfortunately, to the best of my knowledge there is no long read rna data available for sea cucumbers - yet. 

There is however a large amount of rna-seq data for *Apostichopus japonicus* from a number of developmental stages and tissues. This is perfect for gene model prediction. See below for how I mapped this data to several genome assemblies. 

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


# Checking data quality using fastqc 

It's always a good idea to do a quick quality check on the raw data before using it. 
```bash
# creating an environment, installing, and activating fastqc

conda create -n fastqc fastqc
conda activate fastqc

# running fastqc, this will open a gui. Files can be opened from there
fastqc
```

Fastqc results for muscle and bodywall are a little strange, but not worth trimming. Fastqc is showing some over represented sequences, that's not concerning to me as this is rna-seq and not genome data so I expect some over represented sequences. There is the usual 10 bases at the beginning of most reads that are highly variable, that's fine. I could trim that but recent publications have shown that aggressive trimming can mess with mapping results and differential gene expression analysis. So unless the data is particularly bad, I usually do not trim it. 

Some resources regarding whether you should trim/filter rna-seq data:
- [Micheal's  blog](https://www.michaelchimenti.com/2016/06/trim-rna-seq-reads/)   
- [basepairtech.com](https://www.basepairtech.com/blog/trimming-for-rna-seq-data/)    
- [Trimming of sequence reads alters RNA-Seq gene expression estimates](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4766705/)    
- [Read trimming is not required for mapping and quantification of RNA-seq reads at the gene level](https://academic.oup.com/nargab/article/2/3/lqaa068/5901066?login=true)

# Mapping rna-seq data to genome assemblies

Thee are a number of rna-seq mapping tools available. The two popular ones that I am aware of are [STAR](https://academic.oup.com/bioinformatics/article/29/1/15/272537?login=true) and [HiSAT2](https://www.nature.com/articles/nmeth.3317?report=reader). For a more indepth look at what's available and the caveats see this paper titled [Comparison of Short-Read Sequence Aligners Indicates Strengths and Weaknesses for Biologists to Consider](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8087178/)

```bash
mamba create -n star star
conda activate star
```

Most rna-seq mapping tools require the genomes to be indexed. Without getting into the details of how it works, indexes speed up the mapping tools ability to locate scaffolds/contigs in the genome. Without an index, the mapping speed would be dramatically slower. 
See below for how I generated indices using STAR.

Generating genome indexes for the five genome assemblies to be checked

Because I have several genome assemblies to index and I don't want to do manually do it five times, I created a simple bash "for loop". 
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

In ubuntu, the ulimit value for the number of files that can be open at any one time is often too low when trying to run STAR and needs to be set higher. 

Mapping rna-seq reads to the genome assemblies using STAR
```bash
# increasing number of files that can be open at any one time to 100,000
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

While the above command might look complex, it is just a normal STAR command wrapped into a bash "for loop" and added some file naming and bash verbosity to it. A folder is created for each assembly and then each rna-seq file will get its own results folder. 

In hindsight, I probably should have binned the rna-seq results together by tissue type and included the assembly in the result file names. Would have made using multiqc easier. 

Some important details about the above STAR command that I had to figure out the hard way. First is always set --alignIntronMax so something that is reasonable for your species. The default is 100kb. This means that STAR will look 100kb in either direction for putative splice sites and places where the read maps. In mammals this makes sense, in sea cucumbers it does not. The best way to get an idea of what is reasonable is to look at sequencing projects of closely related species and see what the average intron/exon length or gene length was. In sea cucumbers, the average gene length was 5kbp. While theorectically there could be some really long introns, the likelihood is very small. If this parameter isn't modified the results will not only mess up the gene model prediction tools but will make visualizing the mapping results difficult because STAR will find intron sizes far exceeding what would be expected for your species. 

--twopassMode Basic improves the general overall stats and confidence in the mapping results. Generally a good thing to include. 

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

I was hoping it would be able to compile all the results into one graph, but multiqc does not appear to be that sophisticated. It wouldn't be difficult, just tidious, but this could easily be accomplished using excel pivot tables or pandas/R but that's more time than I want to put into this. So I just used multiqc on each assembly directory that I had star mapping results in. 

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