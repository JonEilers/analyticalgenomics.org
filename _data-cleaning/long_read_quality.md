---
title: "Pacbio and Oxford Nanopore data Quality Check"
permalink: /long_read_quality/
layout: single

toc: true 
toc_sticky: true

excerpt: "there be whales"

sidebar:
  nav: sidebar-main


gallery_pacbio:
  - url: /assets/images/data_cleaning/long_read_quality/pacbio_clr.jpeg
    image_path: /assets/images/data_cleaning/long_read_quality/pacbio_clr.jpeg
    title: "Comparison of buscos between genome assemblies"
  - url: /assets/images/data_cleaning/long_read_quality/pacbio_clr_2.jpeg
    image_path: /assets/images/data_cleaning/long_read_quality/pacbio_clr_2.jpeg
    title: "Comparison of buscos between genome assemblies"  
  - url: /assets/images/data_cleaning/long_read_quality/pacbio_clr_3.jpeg
    image_path: /assets/images/data_cleaning/long_read_quality/pacbio_clr_3.jpeg
    title: "Comparison of buscos between genome assemblies"

gallery_nanoplot:
  - url: /assets/images/data_cleaning/long_read_quality/nanoplot/LengthvsQualityScatterPlot_dot.png
    image_path: /assets/images/data_cleaning/long_read_quality/nanoplot/LengthvsQualityScatterPlot_dot.png
  - url: /assets/images/data_cleaning/long_read_quality/nanoplot/LengthvsQualityScatterPlot_kde.png
    image_path: /assets/images/data_cleaning/long_read_quality/nanoplot/LengthvsQualityScatterPlot_kde.png
  - url: /assets/images/data_cleaning/long_read_quality/nanoplot/Yield_By_Length.png
    image_path: /assets/images/data_cleaning/long_read_quality/nanoplot/Yield_By_Length.png


gallery_nanoplot_2:   
  - url: /assets/images/data_cleaning/long_read_quality/nanoplot/Non_weightedLogTransformed_HistogramReadlength.png
    image_path: /assets/images/data_cleaning/long_read_quality/nanoplot/Non_weightedLogTransformed_HistogramReadlength.png
  - url: /assets/images/data_cleaning/long_read_quality/nanoplot/WeightedHistogramReadlength.png
    image_path: /assets/images/data_cleaning/long_read_quality/nanoplot/WeightedHistogramReadlength.png
  - url: /assets/images/data_cleaning/long_read_quality/nanoplot/WeightedLogTransformed_HistogramReadlength.png
    image_path: /assets/images/data_cleaning/long_read_quality/nanoplot/WeightedLogTransformed_HistogramReadlength.png
  - url: /assets/images/data_cleaning/long_read_quality/nanoplot/Non_weightedHistogramReadlength.png
    image_path: /assets/images/data_cleaning/long_read_quality/nanoplot/Non_weightedHistogramReadlength.png



---

# Introduction

Pacbio and Oxford Nanopore technologies sequencing platforms produce long reads. Both platforms have unique challenges with regards to quality control. In this section I will walk through using a variety of tools to assess the quality of reads or raw data generated by these platforms.


# Pacbio Sequence Data

Raw pacbio data can be in two formats. The first is a BAM file containing the raw full-length passes of the sequencing which includes subreads, errors, primers, and the smart bell sequence. The second file format are fastq sequences in which the subreads have been collapsed into consensus sequences and hopefully contain few errors. See below for some images summarizing this. 

{% include gallery id="gallery_pacbio" %}

## Downloading data
 
Pacbio CLR data from a *Stichopus chloronotus* genome sequencing project can be downloaded using the following command.
```bash
# creating conda environment for NCBI's sra tools
conda create -n sratools
conda activate sratools
conda install -c bioconda sra-tools 

# downloading the sra and bam files
# the -X is setting the largest file size to 100 gigabytes
prefetch -X 100G SRR8499555,SRR8499556,SRR8499557,SRR8499558

# this is to extract the fastq files from the sra files
fasterq-dump *.sra
```

Conveniently, this downloads both the compressed fastq files in sra format and the subread.bam files, aka pacbio raw data. However, when running the fastq files through nanoplot or fastqc there are no phred scores.

## SequelTools

Raw pacbio data can be in two formats. The first is a BAM file containing the raw smartbell sequences aligned to themselves. A great tool for checking the stats of the subread bam files is [SequelQC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7532105/). The github page for this tool can be found [here](https://github.com/ISUgenomics/SequelTools)

## Installing sequeltools
which is a pain. I am always surprised to come across well made tools that don't have conda packages available. There is no version associated with the tool. So whatever was availabe on 17 January 2022. 
```bash
# downloading the github respository and making the files executable
git clone https://github.com/ISUgenomics/SequelTools.git
cd SequelTools/Scripts
chmod +x *.sh *.py *.R

# making it callable from the command line
export PATH=$PATH:/home/jon/Working_Files/sea_cuke_species_data/stichopus_chloronotus/SequelTools/Scripts

# creating a conda environment for installing dependencies
conda create -n sequeltools
conda activate sequeltools

conda install -c anaconda python 
conda install -c conda-forge r-base 
conda install -c bioconda samtools 

# my desktop was missing a ubuntu library
sudo apt install libncurses5

# requires a text file containing a list of subread.bam file locations
find $(pwd) -name "*subreads.bam"  > subFiles.txt

# and finally we can run it. 
SequelTools.sh \
  -p a \
  -n 40 \
  -t Q \
  -u /home/jon/Working_Files/sea_cuke_species_data/stichopus_chloronotus/pacbio/sra/SRR8499555/subFiles.txt
```

## Results

# Oxford Nanopore Technologies' Sequence Data

![](/assets/images/resource_images/genome_sequencing/nanopore_dna.jpeg){: .align-right width="400px" height="400px"}  Data created by Oxford Nanopore Technologies platforms is fundamentally different than Pacbio raw data. As a DNA sequence goes through a biological nanopore, it changes an electrical signal which can be interpretted to represent different bases. The resulting data is stored in Fast5 format. Although recent [publications](https://www.nature.com/articles/s41587-021-01147-4) suggest this may change. As there is no fast5 formatted sea cucumber data available on NCBI we will have to settle with analyzing nanopore data that has been "basecalled" already. Meaning an algorithm as interpretted the signal generated by the dna traveling through the pore and written the results to a fastq file. The image to the right gives an example of what this looks like.  

## Downloading data

To download the long read data from NCBI for the genome sequencing project of *Chiridota heheva* see below
```bash
conda activate sratools
prefetch SRR15466781
cd SRR15466781
fasterq-dump *.sra
```
## Nanoplot

The tool that I like to use for investigating the ONT read data is called [Nanoplot](https://academic.oup.com/bioinformatics/advance-article/doi/10.1093/bioinformatics/bty149/4934939) and more information can be found on their github [page](https://github.com/wdecoster/NanoPlot)

## Installing and running Nanoplot
version nanoplot-1.39.0
```bash
conda create -n nanoplot
conda activate nanoplot
conda install -c bioconda nanoplot
```

running nanoplot
```bash
NanoPlot \
	-t 50 \
	-f png \
	--fastq SRR15466781.fastq
```

Note: Nanoplot has a number of options depending on what kind of data it is working with. For example, if your ONT data was basecalled usning Minknow then additional information is stored in the fastq headers which Nanoplot can extract and plot. Unfortunately, this long read data does not have that information. 

## Results

Nanoplot creates a number of images to peruse, even when run using a standard long read fastq file as input. Additionally, it produces some excellent interactive html files. The interactive results can be found [here](/nanoplot_report/). Additionally, it should be noted that the basic nanoplot fastq function can also be used on fastq files from pacbio. 

```bash
Mean read length:                 14,604.5
Mean read quality:                    10.3
Median read length:               10,817.0
Median read quality:                  10.5
Number of reads:               2,905,301.0
Read length N50:                  22,295.0
STDEV read length:                13,118.4
Total bases:              42,430,388,059.0
Number, percentage and megabases of reads above quality cutoffs
>Q5:	2905301 (100.0%) 42430.4Mb
>Q7:	2905299 (100.0%) 42430.4Mb
>Q10:	1914609 (65.9%) 28655.4Mb
>Q12:	126422 (4.4%) 1402.9Mb
>Q15:	7 (0.0%) 0.0Mb
Top 5 highest mean basecall quality scores and their read lengths
1:	15.7 (388)
2:	15.5 (485)
3:	15.3 (308)
4:	15.2 (216)
5:	15.2 (193)
```

So a total of 42 gigabases of data with a median length of 10kb and Q10 phred score. Importantly 28.65 gigabases or 66% of the data had a phred score above Q10. Highest quality reads topped out at Q15 and those were all shorter than 400 bases long. 

As far as data quality goes, that ain't great. It means there is likely an error every ten bases or an accurate base calling rate of ~90%. It is usable? oh ya. Now let's take a look at some of these pretty plots generated by nanoplot. 


{% include gallery id="gallery_nanoplot" %}

Starting left and going right. First plot has average read quality on the Y axis and read length on the X axis. It gives a nice scatter plot of quality vs length. In addition it has histograms of the length and quality. Read length seems to be bimodal - meaning there are two peaks. Quality has a classic bell shape, showing a normal distribution with a slight skew towards higher quality values. There is nothing with a phred score less than 6 so this makes me think the data has been trimmed already. The second graph is a closer look at the previous plot. The far right graph shows the read length vs number of bases. This gives us an idea where the bulk of bases are located - in long vs shorter reads. The rest of the graphs below are close ups of the histograms essentially.

The author of nanoplot has more interesting plots at his [blog](https://gigabaseorgigabyte.wordpress.com/2017/06/01/example-gallery-of-nanoplot/). 


{% include gallery id="gallery_nanoplot_2" %}


