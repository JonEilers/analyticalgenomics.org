---
title: "Identifying and Masking Repetitive Elements in the Genome Assembly"
toc: true
toc_sticky: true
layout: single
permalink: /repeats/

excerpt: “The genome is a book that wrote itself, continually adding, deleting and amending over four billion years.” ~ Matt Ridley

gallery:
  - url: assets/images/annotation/repetitive_elements/TE_distribution_across_life.png
    image_path: assets/images/annotation/repetitive_elements/TE_distribution_across_life.png
    title: Source - A Field Guide to Eukaryotic Transposable Elements - Wells and Feschotte 2020

---

# Introduction

Repetitive elements are integral parts of genomes and can represent a significant majority of a genome assembly. When preparing to identify genes in an assembly it is critical to first identify the repetitive elements. Once they are identified they can be studied further and annotated or "masked" so that the gene model prediction software doesn't misidentify genes found in transpons or gene fragments and other repetitive elements. 

Repetitive elements in a genome consist of two primary classes: [Tandem repeats](https://en.wikipedia.org/wiki/Tandem_repeat) and [transposable elements](https://en.wikipedia.org/wiki/Transposable_element). Tandem repeats such as [satellites](https://en.wikipedia.org/wiki/Satellite_DNA) are unique sequences which are consectively repetititve. One such example that is critical the genome is the [centromere](https://en.wikipedia.org/wiki/Centromere). Transposable elements are ancient genomic "viruses" that are able to replicate themselves and jump around the genome. They are integral to genome evolution and in some instances have been demostrated to be responsible for world changing evolutionary events such as the creation of [placental mammals](https://www.nature.com/articles/s41576-021-00385-1). 

Tranposons can be classified into two major groups: Class I retrotransposons and Class II DNA transposons. Without getting into the gritty details Class I has an rna intermediate and Class II does not. Another important detail is that Class I primarily "jumps" around the genome whereas Class II duplicates itself and the duplicate is inserted elsewhere in the genome.

{% include gallery caption=" Source - [A Field Guide to Eukaryotic Transposable Elements](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8293684/)" %}

Actual distribution of repetitive elements varies wildly between species and even between individuals of the same species due to [transposon expression during embryogenesis](https://rep.bioscientifica.com/view/journals/rep/156/4/REP-18-0218.xml). This variation within individuals of a population can significantly contribute to [adaptation and fitness](https://www.mdpi.com/2073-4425/10/6/419/htm) For this reason a de novo repetitive element "library" is required for non-model organisms prior to identifying and masking repetitive elements in a genome assembly. 

There are two approaches to creating an assembly specific repetitive element library: The quick and dirty method and the correct method. Both require the same first step - using a tool, such as repeatmodeler or [EDTA](https://github.com/oushujun/EDTA), to identify repetitive elements *de novo*. The main difference between the two approaches is that automated tools such as repeatmodeler and EDTA create a lot of false positives. Manual curation of the results is required in order to have complete confidence in the library quality.  Most genome sequencing projects do not have the time and man power to perform that quality control so they often take the results from repeatmodeler/EDTA and directly input them into repeatmasker without any QC. 

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

# Commands

There many many tools for repetitive sequence identification, annotation, masking, etc. By far the most commonly used are RepeatModeler and RepeatMasker. However, in the past few years there has been an effort to create more streamlined and improved tools such as EDTA which output better more information and require less expertise in repetitive elements and bioinformatics. 

## TETools

When using docker to run TETools it mounts the current working directory. So if you will be pulling files from numerous directories you can go to the highest level and run it from there if you don't want to mount abunch of directories. 

```bash
# this sets up the singularity container and allows you to run stuff inside it
sudo ./dfam-tetools.sh

# making a genome assembly database to use with repeatmodeler
BuildDatabase \
    -name ajapmasurca \
    /work/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta

# running repeatmodeler on the ajapmasurca database
RepeatModeler -database database/ajapmasurca -LTRStruct -pa 8

# using the non-curated repeatmodeler output as an input library for repeatmasker
RepeatMasker primary.genome.scf.fasta -xsmall -html -source -gff -pa 10 -lib database/ajapmasurca-families.fa

```

when running either repeatmasker or repeatmodeler keep in mind that the default alignment tool, RMalign, uses 4 cpu threads for each thread you specify. As in, "-pa 10" would use 40 cpu threads. 

For repeatmasker - `-xsmall` changes the bases in an assembly to be all capitals and then changes repetititve elements to lower case. This allows retention of the sequence information but also tells any gene prediction software that the lower case has been "masked" and should be ignored. 

It is also good to use the `-gff` otpion so the resulting predicted repetitive elements can be visualized in a genome browser such as jbrowse2. 

## EDTA

EDTA is intended to be a one stop shop for transposable element identification and annotation. 

```bash
EDTA.pl \
    --genome /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
    --sensitive 1 \
    --anno 1 \
    --evaluate 1 \
    -t 35 \
    --overwrite 0
```

```--sensitive 1``` uses repeatmodeler to help identify any remaining TEs. ```--anno 1``` creates annotations for the identified TEs. ```--evaluate 1``` is a qc step and probably a good idea. 

After setting the parameters and hitting the enter key, sit back and relax. In a bit you will have a masked genome assembly and annotated transposable elements. 

# Results

## Repeatmodeler

RepeatModeler doesn't output any summary files of the results. The most you get is a brief count of how many "families" were found and how long it took. See below for an example. 

```bash
RepeatScout/RECON discovery complete: 2449 families found


LTR Structural Analysis
=======================
  -- Clustering results with previous rounds...
       - 2449 RepeatScout/RECON families
       - 267 LTRPipeline families
       - Removed 83 redundant LTR families.
       - Final family count = 2633
LTRPipeline Time: 01:50:20 (hh:mm:ss) Elapsed Time


Classification Time: 02:29:39 (hh:mm:ss) Elapsed Time


Program Time: 40:46:51 (hh:mm:ss) Elapsed Time
```

However, repeatmodeler outputs a number of files which can be used in downstream analysis. For example, it creates a file containing all the predicted transposon "families". This file can then be manually curated and refined into high confidence families containing sequences that have been trimmed and manually inspected. However, this is time intensive and requires a great deal of knowledge about transposon structure. 

The transponson families file can be directly inputed into repeatmasker without any manual curation and used to hopefully mask most of the repetitive content in a genome assembly, albeit with significiantly higher error rates than if a manually curated family fasta was to be used. 

## RepeatMasker

RepeatMasker outputs your masked genome and a nice summary file of what it found in the genome assembly. This summary file can be quite informative about the quality of your assembly if you already have some background information on what to expect. For example, there are transposons that are species specific such as the CRE/SLACS transposon which is only found in trypanosoma brucei. So you should be raising an eyebrow if some are showing up in a sea cucumber genome. Below I have delved a little deeper into each of the below transposon families, what they are, and what I expect to see in a sea cucumber genome. 

Sources for echinoderm repetitive element composition can be found in genome publications and papers specifically published on transposons. A paper published in 2019 titled [The Transposons of the Sea Urchin Strongylocentrotus intermedius Agassiz, 1863: In Silico Versus In Vitro](https://link.springer.com/article/10.1134/S1063074019060051) does a good job of analyzing transposable elements in sea urchins. While sea urchins are not sea cucumbers, they are related and this paper is probably the best analysis on echinoderm transposable elements. Additionally, the original *A. japonicus* genome publication provides their transposon findings [here](https://doi.org/10.1371/journal.pbio.2003790.s029). Chiridota Heheva genome publication also has some good [graphics](https://www.nature.com/articles/s42003-022-03176-4/figures/2) detailing transposable element landscape in sea cucumbers. 

All of these resources and results have to be taken with a large grain of salt. Unless the genome assembly complete  (as in telomere to telomere) then there will be repetitive element content missing. For example, the genome size of A. japonicus is estimated to be around 900mb. The published genome has a size of ~800mb. A full 100mb of content is missing. Additionally, the contig/scaffold size distribution will be informative about how fragmented the assembly is and assemblies tend to be fragmented at regions containing repetitive elements. So the numbers reported in the papers and below are not accurate, but they can paint a rough picture of the relative proportions of the repetitive elements in the genome and provide a lower bound regarding percentage of the genome that is repetitive elements. 

[A unified classification system for eukaryotic transposable elements](https://www.nature.com/articles/nrg2165)

### Retrotransposons

| Retroelements |               | 84900 | 36669727 bp | 5.8 %  |
|---------------|---------------|-------|-------------|--------|
|               | **SINEs:**        | 4342  | 734663 bp   | 0.12 % |
|               | Penelope      | 15313 | 4664183 bp  | 0.74 % |
|               | **LINEs:**        | 51761 | 19910911 bp | 3.15 % |
|               | CRE/SLACS     | 0     | 0 bp        | 0 %    |
|               | L2/CR1/Rex    | 26022 | 9397503 bp  | 1.49 % |
|               | R1/LOA/Jockey | 0     | 0 bp        | 0 %    |
|               | R2/R4/NeSL    | 0     | 0 bp        | 0 %    |
|               | RTE/Bov-B     | 1890  | 838698 bp   | 0.13 % |
|               | L1/CIN4       | 0     | 0 bp        | 0 %    |
|               | **LTR elements:** | 28797 | 16024153 bp | 2.53 % |
|               | BEL/Pao       | 1598  | 1246057 bp  | 0.2 %  |
|               | Ty1/Copia     | 122   | 169182 bp   | 0.03 % |
|               | Gypsy/DIRS1   | 13852 | 10059761 bp | 1.59 % |
|               | Retroviral    | 201   | 114432 bp   | 0.02 % |

Let's start with the [retrotransposons](https://en.wikipedia.org/wiki/Retrotransposon). As a reminder, these are transposons that have an rna intermediate form. The human genome is about 42% retrotransposon, maize is 49%-78% ()

<details>
  <summary> <strong>SINEs</strong>: Count: 4342 &emsp; Bases: 734663 bp &emsp; Percent: 0.12 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/Sine_structure.png"  caption="Source - Wikipedia." %}

  stuff about SINEs, distribution, and structure
</details>
{: .notice--info}

write about what I would expect for SINEs in the sea cucumber genome

<details>
  <summary> <b>Penelope</b>:  Count: 15313 &emsp; Bases: 4664183 bp &emsp; Percent: 0.74 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/penelope_structure.gif"  caption="Source - Figure 1 from I. Arkhipova 2006 " %}

  Source - <a href="https://academic.oup.com/sysbio/article/55/6/875/1694421?login=false">Distribution and Phylogeny of Penelope-Like Elements in Eukaryotes</a>: ORF structure of Penelope-like elements. The RT moiety consists of the core RT domain that includes the seven highly conserved motifs, followed by the thumb domain and the C-terminal extension. In Penelope-Poseidon–like elements, the GIY-YIG EN domain is immediately adjacent to the C-terminal extension; in Neptune-like and Nematis-like elements, a Zn finger–like domain appears between RT and EN. The long N-terminal extension is characteristic of both Penelope-like RTs and telomerase RTs. Some of the Neptune-like elements may also contain an additional upstream ORF, usually of simple amino acid composition 
  stuff about distribution, and structure
</details>
{: .notice--info}

write about what I would expect for Penelopes in the sea cucumber genome

<details>
  <summary><b>LINEs</b> Count: 51761 &emsp; 	Bases: 19910911 bp &emsp; 	Percent: 3.15 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/line_structure_variation.png"  caption="Source - Deniz et al 2019: ORF1 - open reading frame 1, APE - apurinic endonuclease, RT - reverse transcriptase, EN - endonuclase, TR - terminal repeat " %}

Image is from the paper: <a href = "https://www.nature.com/articles/s41576-019-0106-6">Regulation of transposable elements by DNA modifications </a> <br>
<a href = "https://en.wikipedia.org/wiki/Long_interspersed_nuclear_element">LINE</a> stands for Long Interspersed Nuclear Elements. It conatains a group of non-LTR (long terminal repeat) retrotransposons that are widespread in the genome of many eukaryotes. There are five main groups - L1, RTE, R2, CR1 and Jockey.

  {% include figure image_path="assets/images/annotation/repetitive_elements/line_structure.png"  caption="Source - wikipedia: EN - endonuclease domain, RT - reverse transcriptase, IRES - internal ribosomal entry site, TSD - tandem site duplication, TSS - transcription start site " %}

All LINEs contain at least one open reading frame (ORF2) which codes for a reverse transcriptase (RT) and endonuclease (EN). Many also have a second open reading (ORF1) coding for an additional protein such as Gag-knuckle. The most common LINE in humans is LINE1

</details>
{: .notice--info}

<details>
  <summary><b>CRE/SLACS</b> Count: 0 &emsp; 	Bases: 0 bp &emsp; 	Percent: 0 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>L2/CR1/Rex</b> Count: 26022 &emsp; 	Bases: 9397503 bp &emsp; 	Percent: 1.49 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>R1/LOA/Joecky</b>  	Count: 0 &emsp; 	Bases: 0 bp &emsp; 	Percent: 0 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

 <details>
  <summary><b>R2/R4/NeSL</b>  	Count: 0 &emsp; 	Bases: 0 bp &emsp; 	Percent: 0 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>RTE/Bov-B</b> Count: 1890 &emsp; 	Bases: 838698 bp &emsp; 	Percent: 0.13 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>L1/CIN4</b> Count: 0 &emsp; 	Bases: 0 bp &emsp; 	Percent: 0 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>LTR elements</b>  	Count: 28797 &emsp; 	Bases: 16024153 bp &emsp; 	Percent: 2.53 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>BEL/Pao</b>  	Count: 1598 &emsp; 	Bases: 1246057 bp &emsp; 	Percent: 0.2 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>Ty1/Copia</b> Count: 122 &emsp; 	Bases: 169182 bp &emsp; 	Percent: 0.03 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

 <details>
  <summary><b>Gypsy/DIRS1</b> Count: 13852 &emsp; 	Bases: 10059761 bp &emsp; 	Percent: 1.59 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

<details>
  <summary><b>Retroviral</b>  	Count: 201 &emsp; 	Bases: 114432 bp &emsp; 	Percent: 0.02 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

### DNA transposons

https://github.com/caballero/RepeatLandscape for creating the repeatmasker kimura distance graphs?
repeatmasker has a tool for generating the kimura distance plots: https://github.com/rmhubley/RepeatMasker/blob/master/util/createRepeatLandscape.pl called repeatlandscape

| DNA transposons  |                   | 40162 | 13472419 bp | 2.13 % |
|------------------|-------------------|-------|-------------|--------|
|                  | hobo-Activator    | 17290 | 4162041 bp  | 0.66 % |
|                  | Tc1-IS630-Pogo    | 534   | 120054 bp   | 0.02 % |
|                  | En-Spm            | 0     | 0 bp        | 0 %    |
|                  | MuDR-IS905        | 0     | 0 bp        | 0 %    |
|                  | PiggyBac          | 107   | 61946 bp    | 0.01 % |
|                  | Tourist/Harbinger | 7460  | 1910919 bp  | 0.3 %  |

<details>
  <summary></summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}

### Other transposable elements

|                                     |        |              |         |
|-------------------------------------|--------|--------------|---------|
| Other (Mirage, P-element, Transib)  | 235    | 238763 bp    | 0.04 %  |
| Rolling-circles                     | 1207   | 526201 bp    | 0.08 %  |
| Unclassified:                       | 571376 | 161042674 bp | 25.47 % |
| ***Total interspersed  repeats:***        |        | 211184820 bp | 33.4 %  |

<details>
  <summary></summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  stuff about distribution, and structure
</details>
{: .notice--info}



## EDTA

| Class              |               | Count  | bpMasked  | %masked |
|--------------------|---------------|--------|-----------|---------|
| LTR                |               | --     | --        | --      |
|                    | Copia         | 188    | 170861    | 0.03%   |
|                    | Gypsy         | 17632  | 11882728  | 1.88%   |
|                    | unknown       | 111259 | 32756289  | 5.18%   |
| TIR                |               | --     | --        | --      |
|                    | CACTA         | 80003  | 22098925  | 3.50%   |
|                    | Mutator       | 79045  | 24589315  | 3.89%   |
|                    | PIF_Harbinger | 15340  | 3876612   | 0.61%   |
|                    | Tc1_Mariner   | 5918   | 1626430   | 0.26%   |
|                    | hAT           | 80702  | 34364496  | 5.44%   |
| nonTIR             |               | --     | --        | --      |
|                    | helitron      | 24439  | 7195101   | 1.14%   |
| total interspersed |               | 414526 | 138560757 | 21.92%  |


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