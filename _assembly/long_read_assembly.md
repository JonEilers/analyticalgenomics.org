---
title: "Comparison of three long read genome assembler"
toc: true
toc_sticky: true
layout: single
permalink: /longread_genome_assembly/

gallery:
  - url: /assets/images/busco/busco_analysis.png
    image_path: /assets/images/busco/busco_analysis.png
    title: "Comparison of buscos between genome assemblies"
    caption: 

---

# Introduction

Long reads allow for assembly of repeats and other genome features that short reads are unable to resolve. This can result in a more contiguous genome assembly and better resolution of repetitive elements. However, until recently long reads have suffered from having relatively high errors rates ranging from 10%-1%. While this may not seem high, consider that this means anywhere from 1 to 10 bases in every hundred bases are likely errors. This number greatly increases when looking at the whole genome. However, if the position of the errors are random, then the long reads can be aligned and the "consensus" sequence used for the genome assembly. Unfortunately, current long read sequencing technologies do not contain purely random errors, but it is possible to generate consensus sequences if the sequencing depth is great enough. See below for a few papers worth reading. 

- [Opportunities and challenges in long-read sequencing data analysis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7006217/)
- [Long road to long-read assembly](https://www.nature.com/articles/s41592-021-01057-y)
- [Comparison of long-read methods for sequencing and assembly of a plant genome](https://academic.oup.com/gigascience/article/9/12/giaa146/6042729?login=true#219850953)

To demonstrate some example of long read assembly I selected publicly available data from ncbi. The organism of choice was the sea cucumber Chiridota heheva. The data is from this [preprint](https://www.biorxiv.org/content/10.1101/2021.09.24.461635v1.abstract). The data can be found at this [bioproject](https://www.ncbi.nlm.nih.gov/bioproject?LinkName=sra_bioproject&from_uid=15682756) on NCBI. 

I used my server with 40 physical cores (80 hyperthreaded) and 362gb of ram. The preprint predicted the genome size to be about 1.2gb. The sequencing platform they used was Oxford nanopore for long read and illumina for short read for use in polishing. 

The three assembler chosen were [raven](https://www.nature.com/articles/s43588-021-00073-4), [flye](https://www.nature.com/articles/s41587-019-0072-8), and [shasta](https://www.nature.com/articles/s41587-020-0503-6). These three assemblers were chosen for their speed, accuracy, and efficient resource use. But mostly speed. 

The beauty of most modern genome assemblers is how easy they are to use. While you can tweak a large number of parameters if you want, generally using the default setting is preferable, unless you really know what you are doing or have the time and resources to waste fiddling around with the parameters. Otherwise, all you need is the path to the raw data, path to the output directory and you are good to go. 

# Genome Assembling

## Raven

Raven's github can be found [here](https://github.com/lbcb-sci/raven). They make it pretty easy to install.

```bash
conda create -n raven
conda activate raven
conda install -c bioconda raven-assembler
```

That's it. Easy to install and easy to run, see below for the command. The one caveat is that raven writes the output to the terminal so unless you specify a file you'll end up with your terminal being overrun with sequences. I made this mistake, but thankfully, raven is bloody fast. It took 31083.016350 seconds including two rounds of polishing using racon. This comes out to be 8.6 hours for a 1.2 gigabase genome using 30 cpu cores and 42 gigabases of long read data. 

```bash
raven \
	-t 30 \
	/home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
	>raven_c_heheva.fasta
```

The command line output can be found [here](/raven_output/)

## Shasta

Shasta is fairly easy to set up. However, as of writing this, the authors are not supporting a conda version. Which means the conda version currently available will likely always remain slightly behind the most up to date version of Shasta. Shasta's github can be found [here](https://github.com/chanzuckerberg/shasta). To install, download the latest release that is specific to your operating system [here](https://github.com/chanzuckerberg/shasta/releases/tag/0.8.0). In my case, I use Ubuntu so that is the version I selected. Unpack it and move the folder to wherever you want it. 

To run it on ubuntu I changed directory to where the shasta executable is located and then ran the below command using absolute file paths to the various directories. The latest version requires a config file that contains information about estimated genome coverage, sequencing technology, etc. They suggest using their default config and tweak any important values. I didn't bother tweaking any of the values. 

```bash
./shasta \
	--threads=30 \
	--config /home/jon/Working_Files/shasta/conf/Nanopore-Oct2021.conf \
	--input /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
	--assemblyDirectory /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta \
	--memoryBacking 2M \
	--memoryMode filesystem
```

It took 1.02795 hours to assemble using about 170gb of memory and 30 cores. That is blazingly fast. The commandline output can be found [here](/shasta_output/)

## Flye

And finally, flye. Their github can be found [here](https://github.com/fenderglass/Flye/). Installation is the same as for Raven, that's to say use conda. 

```bash
conda create -n flye
conda activate flye
conda install flye
```

You will need to specify long read data type. Otherwise, just input and output directories and how many threads you have available. 

```bash
flye \
	--nano-raw /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
	--out-dir /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye \
	--threads 30

```

Not the fastest assembler out there, although still faster then canu. It started at 2021-10-08 09:47:05 and finished at 2021-10-11 01:51:56. So a few days. Commandline output can be 
found [here](/flye_output/)

# Summary Statistics

So now that I have three assembly I should do some polishing and quality checking. But for now, I'll leave these basic assembly statistics here. The polishing and QC will happen in a later section.

the bbtools suite has a script  for getting quick genome stats. This happens to be bundled with the bbmap tool on available on conda

```bash
conda create -n bbmap
conda activate bbmap
conda install -c bioconda bbmap 

# running it on a single genome would use stats.sh
stats.sh genome_of_interest

# but I have three genomes, so
statswrapper.sh \
	in=/home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye/assembly.fasta\
	,/home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/raven/raven_c_heheva.fasta\
	,/home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta/Assembly.fasta format=6
```

and here is the result in a nice table format. The top entry is the flye assembly, the middle is raven and the bottom is shasta. Right away there are some obvious and interesting differences.  

| #n\_scaffolds | n\_contigs | scaf\_bp   | contig\_bp | gap\_pct | scaf\_N50 | scaf\_L50 | ctg\_N50 | ctg\_L50 | scaf\_N90 | scaf\_L90 | ctg\_N90 | ctg\_L90 | scaf\_max | ctg\_max | scaf\_n\_gt50K | scaf\_pct\_gt50K | gc\_avg | gc\_std | filename                                                                                                  |
| ------------- | ---------- | ---------- | ---------- | -------- | --------- | --------- | -------- | -------- | --------- | --------- | -------- | -------- | --------- | -------- | -------------- | ---------------- | ------- | ------- | --------------------------------------------------------------------------------------------------------- |
| 16728         | 16728      | 1540823993 | 1540823993 | 0        | 1145      | 307072    | 1145     | 307072   | 7098      | 38787     | 7098     | 38787    | 3605186   | 3605186  | 5989           | 86.823           | 0.36306 | 0.03967 | /home/jon/Working\_Files/Chiridota\_heheva\_genome\_project/genome\_assembly/flye/assembly.fasta          |
| 3227          | 3227       | 1221957363 | 1221957363 | 0        | 623       | 606319    | 623      | 606319   | 2072      | 173820    | 2072     | 173820   | 2901347   | 2901347  | 3133           | 99.716           | 0.36128 | 0.01386 | /home/jon/Working\_Files/Chiridota\_heheva\_genome\_project/genome\_assembly/raven/raven\_c\_heheva.fasta |
| 15199         | 15199      | 1209883028 | 1209883028 | 0        | 1599      | 201222    | 1599     | 201222   | 7021      | 37816     | 7021     | 37816    | 1748484   | 1748484  | 5792           | 85.584           | 0.36212 | 0.04223 | /home/jon/Working\_Files/Chiridota\_heheva\_genome\_project/genome\_assembly/shasta/Assembly.fasta        |

The first is that the flyte assembly is .4gb larger then the other two. The original paper predicted a genome size of 1.2gb using [genomescope](https://academic.oup.com/bioinformatics/article/33/14/2202/3089939) and their assembly size was roughly 1.2gb so this makes me suspect the flye assembler had a difficult time collapsing heterozygous regions. I'll definitely have to take a look at that later. 

The next interesting bit is the number of scaffolds. Shasta and flye had a comparable number, but raven had the least number of scaffolds. Quite impressive. 

Next are the genome summary statistics. Note: the L50 and N50 results are switched here, probably a mistake in the tool output. The raven assembly has the highest N50, which makes sense if it is indeed the most contigious. So half the scaffolds are shorter than 600kbp. The l50 is 623, so half the genome is contained in 623 reads, these are going to be the longest reads. 

Let use another summary stats tool to get a better sense of the distribution of the raven genome: genometools. 

```bash
conda create -n genometools
conda activate genometools
conda install -c bioconda genometools-genometools 

gt seqstat raven_c_heheva.fasta 
# number of contigs:     3227
# total contigs length:  1221957363
# mean contig size:      378666.68
# contig size first quartile: 129746
# median contig size:         248456
# contig size third quartile: 509682
# longest contig:             2901347
# shortest contig:            7773
# contigs > 500 nt:           3227 (100.00 %)
# contigs > 1K nt:            3227 (100.00 %)
# contigs > 10K nt:           3223 (99.88 %)
# contigs > 100K nt:          2709 (83.95 %)
# contigs > 1M nt:            207 (6.41 %)
# N50                606319
# L50                623
# N80                270244
# L80                1511

```

So most of the genome is contained in scaffolds longer than 10kbp with all but 500 scaffolds being longer than 100kbp. That looks pretty darn good. To get a better sense of the quality of these genomes I'll need to take a closer look using tools such as mummer4 to compare them or blobtoolkit and merqury to check for completeness and missassemblies. I'll do this in a later section. 