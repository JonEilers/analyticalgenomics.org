---
title: "Identifying and Masking Repetitive Elements in the Genome Assembly"
toc: true
toc_sticky: true
layout: single
permalink: /repeats/

excerpt: “The genome is a book that wrote itself, continually adding, deleting and amending over four billion years.” ~ Matt Ridley


---

# Introduction

Repetitive elements are integral parts of genomes and can represent a significant majority of a genome assembly. When preparing to identify genes in an assembly it is critical to first identify the repetitive elements. Once they are identified they can be studied further and annotated or "masked" so that the gene model prediction software doesn't misidentify genes found in transpons or gene fragments and other repetitive elements. 

Repetitive elements in a genome consist of two primary classes: [Tandem repeats](https://en.wikipedia.org/wiki/Tandem_repeat) and [transposable elements](https://en.wikipedia.org/wiki/Transposable_element). Tandem repeats such as [satellites](https://en.wikipedia.org/wiki/Satellite_DNA) are unique sequences which are consectively repetititve. One such example that is critical the genome is the [centromere](https://en.wikipedia.org/wiki/Centromere). Transposable elements are ancient genomic "viruses" that are able to replicate themselves and jump around the genome. They are integral to genome evolution and in some instances have been demostrated to be responsible for world changing evolutionary events such as the creation of [placental mammals](https://www.nature.com/articles/s41576-021-00385-1). 

Tranposons can be classified into two major groups: Class I retrotransposons and Class II DNA transposons. Without getting into the gritty details Class I has an rna intermediate and Class II does not. Another important detail is that Class I primarily "jumps" around the genome whereas Class II duplicates itself and the duplicate is inserted elsewhere in the genome.

Actual distribution of repetitive elements varies wildly between species and even between individuals of the same species due to [transposon expression during embryogenesis](https://rep.bioscientifica.com/view/journals/rep/156/4/REP-18-0218.xml). For this reason a de novo repetitive element "library" is required for non-model organisms prior to identifying and masking repetitive elements in a genome assembly. 

There are two approaches to creating an assembly specific repetitive element library: The quick and dirty method and the correct method. Both require the same first step - using a tool, such as repeatmodeler or [EDTA](https://github.com/oushujun/EDTA), to identify repetitive elements *de novo*. The first difference between the two approaches is that automated tools such as repeatmodeler and EDTA require manual curation of the results in order to have complete confidence in the library quality. Most genome sequencing projects do not have the time and man power to perform that quality control so they often take the results from repeatmodeler and directly input them into repeatmasker without any QC. 

With that in mind I will cover the manual curation of transposable elements in a separate section and focus primarily on generating repetitive elements libraries and masking a genome assembly in this section. 

Two fantastic tools for generating de novo libraries are [EDTA](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1905-y) and [repeatmodeler](https://www.pnas.org/doi/abs/10.1073/pnas.1921046117). For masking a genome assembly, [RepeatMasker](https://repeatmasker.org/) is a stalwart. See below for installation. 

# Installation


EDTA includes tools such as repeatmodeler and repeatmasker and is intended to be a standalone repeat identification, annotation, and masking tool. More information can be found at EDTA [github](https://github.com/oushujun/EDTA#introduction).

EDTA is also available for docker/singularity. For this walk through I use mamba to install it into a conda environment. 
```bash
# installing EDTA
mamba create -n edta -c conda-forge -c bioconda edta python=3.6 tensorflow=1.14 'h5py<3'
```

Additionally, the DFAM consortium has created a docker/singularity image containing the minimum requirements for identifying and masking repetitive elements in a genome assembly. Installing repeatmasker and repeatmodeler and their dependencies can be a headache so this docker image is a useful way to save yourself the pain. More information is available at the Dfam-consortium's [TETools github](https://github.com/Dfam-consortium/TETools)

```bash
# downloads the scripts
curl -sSLO https://github.com/Dfam-consortium/TETools/raw/master/dfam-tetools.sh

# makes the script executable
chmod +x dfam-tetools.sh

# runs the script which pulls the container image and runs it
./dfam-tetools.sh 
```

It is preferred to run this container using singularity, however docker will work just as well.  

# Running Tetools

When using docker to run TETools it mounts the current working directory. So if you will be pulling files from numerous directories you can go to the highest level and run it from there if you don't want to mount abunch of directories. 

```bash
# this sets up the singularity container and allows you to run stuff inside it
sudo ./dfam-tetools.sh

# once in use the below commands
BuildDatabase \
    -name ajapmasurca \
    /work/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta

# running repeatmodeler on the ajapmasurca database
RepeatModeler -database database/ajapmasurca -LTRStruct -pa 8

# using the non-curated repeatmodeler output as an input library for repeatmasker
RepeatMasker genome1.fa [-lib library.fa] -pa 8

RepeatMasker primary.genome.scf.fasta -xsmall -html -source -gff -pa 10 -lib database/ajapmasurca-families.fa

# repeatmasker didn't like the length of some scaffold names so I used regex in sublime to rename some. See the regex pattern I used below
(?<header>>scf.*):F:.*

```

when running either repeatmasker or repeatmodeler keep in mind that the default alignment tool, RMalign, using 4 cpu thread for each thread you specify. As in, "-pa 10" would use 40 cpu threads. 


```

Repeatmodeler results
```bash
Processing RECON family: 6233
  - Saving 16 elements
  - Refining family-6233 model...
Family Refinement: 03:50:14 (hh:mm:ss) Elapsed Time
Round Time: 26:36:57 (hh:mm:ss) Elapsed Time

RepeatScout/RECON discovery complete: 2449 families found


LTR Structural Analysis
=======================
Running LtrHarvest...     : 00:34:31 (hh:mm:ss) Elapsed Time
Running Ltr_retriever...  : 00:31:58 (hh:mm:ss) Elapsed Time
Aligning instances...     : 00:24:46 (hh:mm:ss) Elapsed Time
Clustering...             : 00:00:12 (hh:mm:ss) Elapsed Time
Refining families...      : 00:16:17 (hh:mm:ss) Elapsed Time
Program Time: 01:47:44 (hh:mm:ss) Elapsed Time
  -- Clustering results with previous rounds...
       - 2449 RepeatScout/RECON families
       - 267 LTRPipeline families
       - Removed 83 redundant LTR families.
       - Final family count = 2633
LTRPipeline Time: 01:50:20 (hh:mm:ss) Elapsed Time


RepeatClassifier Version 2.0.3
======================================
Search Engine = rmblast
  - Looking for Simple and Low Complexity sequences..
  - Looking for similarity to known repeat proteins..
  - Looking for similarity to known repeat consensi..
Classification Time: 02:29:39 (hh:mm:ss) Elapsed Time


Program Time: 40:46:51 (hh:mm:ss) Elapsed Time
Working directory:  /work/japonicus_genome_project/repeats/tetools/RM_79.MonMay160329402022
may be deleted unless there were problems with the run.

The results have been saved to:
  database/ajapmasurca-families.fa  - Consensus sequences for each family identified.
  database/ajapmasurca-families.stk - Seed alignments for each family identified.
  database/ajapmasurca-rmod.log     - Execution log.  Useful for reproducing results.


```



# Running EDTA

```bash
EDTA.pl \
    --genome /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
    --sensitive 1 \
    --anno 1 \
    --evaluate 1 \
    -t 35 \
    --overwrite 0
```

## Viewing results

download jbrowse2

creating genome assembly index using samtools
```bash
mamba create -n samtools samtools
conda activate samtools

samtools faidx primary.genome.scf.fasta
```

# results

# References

[Tools and databases for solving problems in detection and identification of repetitive DNA sequences](https://hrcak.srce.hr/254644)

[Pattern of Repetitive Element Transcription Segregate Cell Lineages during the Embryogenesis of Sea Urchin Strongylocentrotus purpuratus](https://www.mdpi.com/2227-9059/9/11/1736)

[Benchmarking transposable element annotation methods for creation of a streamlined, comprehensive pipeline](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1905-y)

[Superior ab initio identification, annotation and characterisation of TEs and segmental duplications from genome assemblies](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0193588)

[Computational tools to unmask transposable elements](https://www.nature.com/articles/s41576-018-0050-x)

[Transposable Elements: Classification, Identification, and Their Use As a Tool For Comparative Genomics](https://link.springer.com/protocol/10.1007/978-1-4939-9074-0_6)

## Resources

[Methodologies for the De novo Discovery of Transposable Element Families](https://pubmed.ncbi.nlm.nih.gov/35456515/)

[Benchmarking transposable element annotation methods for creation of a streamlined, comprehensive pipeline](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1905-y)

[Transposable element annotation in non-model species: The benefits of species-specific repeat libraries using semi-automated EDTA and DeepTE de novo pipelines](https://pubmed.ncbi.nlm.nih.gov/34407282/)

[RepetDB: a unified resource for transposable element references](https://mobilednajournal.biomedcentral.com/articles/10.1186/s13100-019-0150-y)

[TE Density: a tool to investigate the biology of transposable elements](https://pubmed.ncbi.nlm.nih.gov/35413944/)


[A beginner’s guide to manual curation of transposable elements](https://mobilednajournal.biomedcentral.com/articles/10.1186/s13100-021-00259-7)

[Curation guidelines for de novo generated transposable element families](https://dfam.org/assets/publications/2021-Current_Protocols-Curation_guidelines.pdf)

[TE-aid](https://github.com/clemgoub/TE-Aid)
[TE Hub](https://tehub.org/)

[Ten things you should know about transposable elements](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-018-1577-z)

[TransposonUltimate: software for transposon classification, annotation and detection](https://academic.oup.com/nar/advance-article/doi/10.1093/nar/gkac136/6541023?login=false)

## tools
tephra, PiRATE, Maker-P, RepeatModeler, EDTA, REPET