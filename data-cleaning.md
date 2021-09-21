---
title: "Sequence Data Analysis and QC"
permalink: /data-cleaning/
layout: collection
sidebar:
  nav: sidebar-main
classes: wide
entries_layout: grid

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Garbage in, garbage out."


gallery:
  - url: /fastqc/
    image_path: /assets/images/blobtoolkit/californicus_masurca.snail.png
    title: Apostichopus californicus Masurca Assembly
---
 Each sequencing technology introduces it own unique artifacts and inaccuracies into the sequencing data. Identifying, removing the artifacts, and then analyzing the raw data are the first steps in preparing the sequence data for assembly. 

 Let's start from the beginning. When you first acquire your data whether it is via a sketchy flashdrive or downloaded from the internet, you will want to check the data integrity. The easiest way is to perform something called a [checksum](https://en.wikipedia.org/wiki/Checksum). This essentially checks that the calculated hash value for the copied data is the same as the calculated hash value for the original data, this verifies that the copied data has not been corrupted. The popular algorithm to use is [md5](https://en.wikipedia.org/wiki/MD5) and is fairly easy. See below link below for a more indepth look at how to do this. 

-  ### [Using Md5sum to verify data integrity](/checksum/)

Once you can have confidence in the data integrity, it's time to get some rough ideas about what the data looks like. A few things need to be checked: basic statistics such as number of reads and read length, adapter contamination, read quality, overrepresented sequences etc. Thankfully, there is a very popular tool for doing this easily called Fastqc. Fastqc works with any Fastq file, be it pacbio, illumina, or nanopore sequence data. See below for a link to a page that goes into more detail on using Fastqc, adapters, and the raw data report Novogene provided with the data. 

- ### [Checking data quality using FastQC](/fastqc/)

In some cases, there may be problems with the raw sequence data and it needs some cleaning. This can be due to excessive adapter contamination, way too many duplicated sequences, a significiant proportion of the reads are low quality, etc. It is also important to consider how these reads will be used. In some cases you may be able to get away with simply ignoring read quality problems. There are a handful of publications that have looked at how read trimming impacts computation downstream in the pipeline such as during [genome assembly](https://www.mdpi.com/2073-4425/10/10/737). A [paper](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0085024) from 2013 looked at how trimming impacts different data types and their ultimate uses. In this paper, they note that read trimming negatively impacts assembly quality if the trimmming was performed on a high quality dataset. Additionally, some tool specifically say to not perform read trimming. This is the case with the genome assembler [MaSURCA](https://github.com/alekseyzimin/masurca). In my particular case, the raw Illumina data was already high quality so I did nothing to that dataset. However, the Oxford Nanopore data was terrible. See below for further insight and instruction on how to perform read trimming. 

- ### [Raw sequence data trimming and filtering](/trimming/)

Now the data is looking good, all the reads have high phred scores and there appears to be little to no adapter contamination. It is time to take a closer look at the raw sequence data. There are a number of tools for doing this, most relay on the analysis of [K-mers](https://en.wikipedia.org/wiki/K-mer) to estimate different aspects of the data. K-mers are essentially sub-sequences or sub-strings and the distribution of these sub-sequences in the data can be used to estimate parameters. One example is in [choice of k-mer size](https://en.wikipedia.org/wiki/K-mer#Choice_of_k-mer_size) when assembling genomes. Larger k-mer sizes result in more memory usage, but improved genome assembly contiguity. Thus it can be important to find the optimal k-mer size for genome assembly. [Kmergenie](http://kmergenie.bx.psu.edu/) can be used to do this. See below for an example. 

- ### [Estimating Best Genome Assembly K-mer size using Kmergenie](/kmergenie/)


However, some genome assemblers, such as [MaSURCA](https://github.com/alekseyzimin/masurca) do not require a k-mer size as input, but rather requires an estimated genome size. If there is no genome size estimate available, the genome size can be estimated using k-mer analysis. See below for an example.

- ### [Estimating genome characteristics using Jellyfish and GenomeScope](/genomescope/)


Additionally, k-mer analysis can be used to check for various types of contamination in the raw sequence data. The [K-mer Analysis Toolkit (KAT)](https://kat.readthedocs.io/en/latest/index.html) can be used to check for GC bias which might indicate different kinds of contamination such as from PCR. KAT has a number of differents for analyzying the k-mer distribution and what it might indicate. See below for some examples 
- ### [K-mer Analysis using KAT](/kat/)

If everything looks good or has been cleaned up, the next step is [genome assembly](/assembly/)! 

