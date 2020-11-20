---
title: "Raw Sequence QC"
permalink: /sequenceQC/
toc: true
toc_label: "Sequence Quality Control"
toc_sticky: true
---

![](/assets/images/resource_images/sequenceQC/fastqc.png){: .align-right width="500px" height="300px"} Taking a quick peak at your data is important before spending countless days and weeks on the important bits. Below are a list of tools for performing quality control of sequence data. All of these tools can be downloaded using [conda](https://docs.conda.io/en/latest/). I also want to note that in my trawling of papers and information on genome assembly and rna-seq mapping that the current consensus of actual experts is don't trim your reads. The tools for mapping, aligning, and assembling are better at correcting errors from untrimmed data than we are at trimming the reads correctly. 

### Sequence Data Quality Analysis Tools

[FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) is a tool for assessing raw sequence quality.

[Kat](https://www.earlham.ac.uk/kat-tools) is a suite of tools for generating, analyzing, and comparing k-mer spectra produced from sequence data. Spectra includes determing sequence completeness, sequence bias, identifying contaminants, validating assemblies. 

[Jellyfish](https://www.cbcb.umd.edu/software/jellyfish/) is a tool for counting k-mers in sequence data

[GenomeScope](http://qb.cshl.edu/genomescope/) is both a R package and website which estimates genome heterozygosity, repeat content, and size using kmer based statistical approaches

[kmergenie](http://kmergenie.bx.psu.edu/) is a tool for estimating the best k-mer length for de novo genome assembly

[MultiQC](https://multiqc.info/) searches a given directory for analysis logs and compiles a HTML report. It's a general use tool, perfect for summarising the output from numerous bioinformatics tools.

### Sequence Data Filtering Tools

[TrimAL](http://trimal.cgenomics.org/) is a tool for the automated removal of spurious sequences or poorly aligned regions from a multiple sequence alignment

[Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) is a flexible read trimming tool for Illumina NGS data. I don't like it. 

[AfterQC](https://github.com/OpenGene/AfterQC) does automatic filtering, trimming, error removing and quality control for fastq data

[AdapterRemoval](https://github.com/MikkelSchubert/adapterremoval) searches for and removes adapter sequences from High-Throughput Sequencing (HTS) data and (optionally) trims low quality bases from the 3' end of reads following adapter removal. 

[Trim Galore!](https://github.com/FelixKrueger/TrimGalore) is a wrapper around Cutadapt and FastQC to consistently apply adapter and quality trimming to FastQ files, with extra functionality for RRBS data. 

[BBDuk](https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/bbduk-guide/):  “Duk” stands for Decontamination Using Kmers. BBDuk was developed to combine most common data-quality-related trimming, filtering, and masking operations into a single high-performance tool. It is capable of quality-trimming and filtering, adapter-trimming, contaminant-filtering via kmer matching, sequence masking, GC-filtering, length filtering, entropy-filtering, format conversion, histogram generation, subsampling, quality-score recalibration, kmer cardinality estimation, and various other operations in a single pass. Specifically, any combination of operations is possible in a single pass, with the exception of kmer-based operations (kmer trimming, kmer masking, or kmer filtering)

