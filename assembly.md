---
title: "Genome Assembly"
permalink: /assembly/
layout: single

header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "Assembling a genome is like putting together a million piece puzzle"

toc: true 
toc_sticky: true

sidebar:
  nav: sidebar-main

---

# Please note 
This page is currently under construction with new content and mistakes added regularly.

# Introduction

Genome Assembly is a [hard](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2005894). Even now with long read sequencing, [there is still a long ways to go](https://www.nature.com/articles/s41592-021-01057-y). In 1999, the human genome sequence was announced. In 2021, a telomere to telomere genome assembly, with no gaps, was announced. Even with the advances in technology and reduction in cost, many genomes cannot realistically be sequenced and assembled without a significant investment. For example, most salamander and newt species have genomes that are estimated to be 100 gigabase. The largest genome sequenced so far was the [lungfish](https://www.nature.com/articles/s41586-021-03198-8?#Sec2), with a genome size of 43 gigabases. Lungfish have a diploid genome, which is fairly normal for a large animal. But plants and many other organisms are not diploid and require extra caution when assembling. One notable example is [wheat](https://www.sciencedirect.com/science/article/pii/S1672022920300590) which has chromosomes from three different genomes and is an [allohexaploid](https://www.nature.com/articles/s41586-020-2961-x0), meaning it has six sets of two chromsomes. Most genome assembler would not know what to do with this and would likely collapse the three sets in a psuedohaploid assembly, loosing a great deal of information in the process. 

Genome assembly approaches largely depend on the type and quality of data you have available to you. Sanger sequencing used to be the golden standard. Then Illumina shotgun sequencing took over for awhile. Now we have long read sequencing using either PacBio or Oxford Nanopore Technologies platforms. Additional wet lab techniques such as [Hi-C](https://en.wikipedia.org/wiki/Chromosome_conformation_capture#Hi-C_(all-vs-all)) or [optical mapping](https://en.wikipedia.org/wiki/Optical_mapping) can further bring the assembly pieces together into chromosome length scaffolds. Any combination of the above can be found in most current genome assembly papers. However, if Illumina is all you have and genomes of closely related species are available, then reference assisted genome assembly is a possibility, but with caveats. 

# Assembly

## Short Read

Illumina sequencing is cheap, relatively speaking. In my case, a mere $1200 got me 100X coverage of the sea cucumber genome. While Illumina data won't get you chromosome length scaffolds, what it will get you is access to the majority of the gene space in a genome. With some caveats of course. You likely won't be seeing a complete [Titin](https://en.wikipedia.org/wiki/Titin) gene and gene counts will likely be inaccurate as a result of gene fragmentation. For example, in my short read sea cucumber genome, the telomerase gene was split into two separate genes. However, if you're interested in shorter genes such as [Mortalin](https://en.wikipedia.org/wiki/HSPA9) or highly conserved genes such as [Survivin](https://en.wikipedia.org/wiki/Survivin) then you'll probably be just fine with a short read assembly. If you are interested studying gene regulation and how repetitive elements influence it, you will want chromosome length scaffolds. 

It has been close to two decades since Illumina short read sequencing became accessible and as a result there are innumerable short read assemblers. In recent years though, significant progress has been made in assembly algorithms and the result has been some blazing fast and accurate assembly tools. See below for two that I am partial towards.

- [MaSuRCA](/masurca/)

In my case, I am working with organisms that are highly heterozygous. What this means is there is enough variation between the two chromosomes in each chromosome pair to cause problems for a genome assembler that doesn't take this into account. Platanus-Allee was specifically designed to deal with that. The author also created another tool for genomes that have less variation (such as humans) called [Platanus](http://platanus.bio.titech.ac.jp/platanus-assembler). On the Platanus-Allee webpage they state "for low heterozygous species genome (as a guide < 1.0 %), Platanus assembler would mark better performance than Platanus-allee.

- [Platanus-Allee](/platanus-allee/)

Also worth noting is that both the above assemblers have the ability to work with long reads, and in MaSuRCA's case, reference genomes.

Note to self: 
The [Australostichopus mollis](https://www.ncbi.nlm.nih.gov/bioproject/PRJEB10682/), [Apostichopus leukothele](https://www.ncbi.nlm.nih.gov/sra/SRX8086344[accn]) and [Actinopyga echinites](https://www.ncbi.nlm.nih.gov/assembly/GCA_010015985.1#/st) genomes are some candidates for future genome analysis and reassembly.

## Long Read

With the advent of "long-read" sequencing technology, the genome sequencing landscape dramatically changed. It became possible to acquire chromosome long scaffolds with repetitive elements mostly resolved. While certain regions of chromosome were still difficult, such as telomeres and centromeres, the vast majority of the genome was now available for assembly.

While the current state of genetics leaves most researchers interested in the "gene space" of the genome, it has becomes increasingly obvious that the regulation of these genes and consequently the expression and phenotype are controlled not by the genes themselves but by promoter and enhancer sequences present in the hard to resolve areas of the genomes. In the last few years [modeling](https://www.nature.com/articles/s41467-021-25875-y) and [experiments](https://www.sciencedirect.com/science/article/abs/pii/S0959437X2030037X) have also revealed the critical role chromatin structure plays in gene expression. This is all to say, it's really useful and important to have a contiguous genome and this is now achievable using long read sequencing technology. 

Currently, there are long read sequencing technologies that are duking it out: Pacbio and Oxford Nanopore Technologies. There are dozens of papers arguing about the virtues and sins of each and I won't go into it here. However, [this](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-1935-5) paper does offer a good introduction and insight into the pros and cons of both. 

Now on to long read sequence genome assembly. Because the reads are longer, but contain more errors, genome assemblers use slightly different approaches to handling the "noisy" reads. But the results are usually better then anything achievable using short read data and most genome assembly pipelines include a few rounds of "polishing" post genome assembly. I go into more detail on this further down this page. See the link below for re-assembly of a sea cucumber genome using three different long read assemblers. 

- [Chiridota heheva reassembly using Raven, Flye, and Shasta assemblers](/longread_genome_assembly/)

## Hybrid

__In Progress__

Future pages and content that will be included here are:
- hybrid long read/short read assembly using platanus-allee, haslr, and masurca of *Apostichopus japonicus* and *Apostichopus parvimensis* genomes
- incorporation of hi-c data to achieve chromosome scale scaffolds using salsa and hirise on the [sea urchin Lytechinus variegatusgenome](https://academic.oup.com/gbe/article/12/7/1080/5841217?login=true)
- examples from papers such as this [one](https://www.biorxiv.org/content/10.1101/2020.07.01.182477v1
) on coral endosymbiont genome assembly.

## Reference Assisted

__In Progress__

Future content to be found here includes:
- reference assisted genome assembly using tools such as redundans, masurca, ragout, ragtag, progressive cactus, etc. 
- content explaining the pitfalls and benefits of using reference assisted genome assembly
- example: [Redundans](/redundans/)

## Organelle Assembly

__In Progress__

Future content will include:
- how to assemble plastid genomes 
- examples of tools such as [GetOrganelle](https://www.biorxiv.org/content/10.1101/256479v3.abstract)

# Assembly QC

Once an a few assemblies are complete, it's time to determine which one is best and if any further steps need to be taken. There are a few approaches to this each approach has a great tool for it. 

## The N50 and summary statistics
A standard metric for genome contiguity is the N50 value. N50 is a tricky beast to understand and I seen more blogs and descriptions get it wrong then right. Thankfully, [wikipedia](https://en.wikipedia.org/wiki/N50,_L50,_and_related_statistics#N50) gets it right. Without getting into the details on it, the thing that matters when considering the N50 is that the majority of reads in an assembly will be shorter than the N50 value. If you have an N50 of 9kb, then the majority of the assembly will be scaffolds or contigs shorter than 9kb. Having an N50 of 9kb consequently means gene prediction will likely capture the bulk of the genes, but there will likely be a large number of fragmented genes such as [titin](https://en.wikipedia.org/wiki/Titin). While there are a number of tools for acquiring this metric, probably my favorite way to visualize it is the snail plot produced via [Blobtoolkit](https://www.g3journal.org/content/10/4/1361). [Here](https://blobtoolkit.genomehubs.org/) is a link to their website. See the link below for an example. 

- [Summary statistics via Blobtoolkit](/blobtoolkit/)

## Assembly quality assessment using BUSCOs
An easy to capture the level of fragmentation and also get idea of what to expect when predicting genes is to check the [BUSCOs](https://pubmed.ncbi.nlm.nih.gov/26059717/). You'll hear BUSCOs thrown around a lot in genome papers and among scientists involved in genome sequencing. It's treated like a holy metric for how good your assembly is and it is a reasonable way to check. In essence, someone took the time to find a number of genes that are highly conserved across the kingdoms and phylum of life. Because of their conserved nature, it is a reasonable assumption that the genome of your organism likely contains these genes. So if the majority of these genes can be found in your assembly and they are not fragmented or unexpectedly duplicated, then it is reasonable to assume that a similar percentage of genes in the genome will likewise be in good shape. It is important to understand though, that BUSCO results do not represent a best case scenario but rather a targeted random sample of the genome assembly. See below for an example

- [Assembly quality assessment using BUSCO analysis](/busco/)

## Checking for contamination in the assembly
In addition to looking at summary statistics and checking BUSCOs, it is also wise to check for contamination. During the sequencing process, DNA from other organisms may be in the sample and it's important to know if that has found its way into the genome assembly. A common method for checking this is to download a uniprot or refseq protein database and blast it against your assembly then check to see what organisms had the highest hit. If those organisms are closely related to the organism of interest, then it is safe to say that's probably solid, but if there are a lot of hits for distantly related organisms, then it might be a good idea to consider preprocessing and filtering the raw data before assembling. [Blobtoolkit](https://www.g3journal.org/content/10/4/1361) produces two different types of graphs, the blobplot and the Cumulative assembly span plot, for visualizing this. 

- [Assembly contaminiation via Blobtoolkit](/blobtoolkit/)

## K-mer analysis of the assembly
Another way to assess the quality of the assembly and to check for contamination is using K-mers. Truly a versatile way to sequence data. One way to check assembly quality using K-mers is by see how many unique K-mers are found in both the assembly and the raw data then visualize it using a [K-mer spectra graph](https://academic.oup.com/view-large/figure/118668344/btw663f1.tif). There are currently two excellent tools for this: [KAT](https://academic.oup.com/bioinformatics/article/33/4/574/2664339?login=true) and [Merqury](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9). To see a feature comparison between the two, click [here](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9/tables/1). See the below links for examples. 

- [Assembly contamination assessment using KAT](/kat_assembly/)

- [K-mer completeness and consensus quality assessment of genome assemblies](/merqury_assembly/)

## Assembly comparisons
Most genome assembly projects try using several different assembly tools and it can be useful to check for differences between them. Additionally, It can be useful to see how many changes were made when gap closing or polishing. Either of those things can be accomplished using a tool called [mummer4](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005944). Mummer4 produces a "delta" file that can then be used in other analysis tools such as [Nucdiff](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-017-1748-z) or visualization tools such as [Assemblytics](https://academic.oup.com/bioinformatics/article/32/19/3021/2196631). It can also output a summary file containing basic counts of indels and other differences in addition to a bam file for loading into a genome browser such as [Ribbons](https://genomeribbon.com/) if a closer look is required. 

- [Comparing assemblies using Mummer4](/mummer4/)

#  Polishing and gap closing

__In Progress__

Once you have an assembly that is as good as it will get, it might be possible to squeeze a little more out of your data using gap closing and polishing tools. However, just like with read trimming, doing either gap closing or polishing can result in an assembly that was worse then what you started with. I also want to add that overzealous use of gap closing can result in extremely poor assemblies. This is a huge problem when these assemblies are then uploaded into NCBI and used as references genomes for other projects. Most researchers do not have the skill, knowledge, or time to check that the assembly or genes from assemblies are trustworthy, potentially resulting in a lot of frustration and wasted time and money. So proceed with caution. Click on the below link for examples of gap closing gone wrong or genome assemblies that look good when looking at just summary statistics but don't hold up when viewed from other angles. 

- [Gap closing gone wrong and misleading genome assemblies](/misleading_genomes/)

Using long read data, it is now possible to close gaps that are produced by genome assembler with a high degree of confidence. This is significantly different from previous tools such as [SSpace](https://academic.oup.com/bioinformatics/article/27/4/578/197626) that relied on paired end short reads to close gaps or extend contigs. There are two problems with this approach, the obvious one is that the reads are too short to accurately span repetitive elements. The second problem is that these tools are haplo-type insensitive, meaning they can't tell if they are actually extending a real contig or just stringing alleles together creating inaccurate duplications. 

- [Gap closing using Dentist]()
- [Genome assembly polishing using hapo-g]()

Recently, researcher who completed the first 100% complete genome assembly uploaded a [pre-print]((https://www.biorxiv.org/content/10.1101/2021.07.02.450803v1)) to biorxiv detailing their process and advocating for a more manual gap closing process. This is a neat paper and their process will likely feature in many future efforts to create 100% complete telomere to telomere genome assemblies. Although, this process is likely to primarily be used on genome assemblies that are already near completion. Sea cucumber genomes may be waiting awhile before they get this level of treatment. The paper is titled [chasing perfection](https://www.biorxiv.org/content/10.1101/2021.07.02.450803v1).

__To Do__

[SSpace](https://academic.oup.com/bioinformatics/article/27/4/578/197626) don't use this. ugh. maybe work up an example of why? 
[github of last opensource version](https://github.com/nsoranzo/sspace_basic)

hmmm maybe play with [AGB](https://github.com/almiheenko/AGB
) for visualizing long read assembly?
