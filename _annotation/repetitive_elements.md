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

gallery_2:
  - url: assets/images/annotation/repetitive_elements/echinoderm_TE.webp
    image_path: assets/images/annotation/repetitive_elements/echinoderm_TE.webp
    title: Figure 2 - Landscape of transposable elements in echinoderm genomes - Zhang et al 2022

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

Before I dive into brief explanations of each row in the repeatmasker results - please note that the current classification schemes for repetitive elements is a complete mess. Each paper has its own preference with how to display the groups and their relationship. For example, it will require really digging into the literature in order to figure out the what L2, CR1, and Rex elements have in common and why they are grouped together. When reading other papers, L2 and CR1 won't even show up, etc. That is the struggle when dealing with repettitive element classification. For the sake of this section I have stuck with what repeatmasker outputs. 

[A unified classification system for eukaryotic transposable elements](https://www.nature.com/articles/nrg2165)

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


 {% include gallery id="gallery_2" caption="Figure 2 - Landscape of transposable elements in echinoderm genomes - Zhang et al 2022" %}

### Retrotransposons

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

</details>
{: .notice--info}

write about what I would expect for Penelopes in the sea cucumber genome

<details>
  <summary><b>LINEs</b> Count: 51761 &emsp; 	Bases: 19910911 bp &emsp; 	Percent: 3.15 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/line_structure_variation.png"  caption="Source - Deniz et al 2019: ORF1 - open reading frame 1, APE - apurinic endonuclease, RT - reverse transcriptase, EN - endonuclase, TR - terminal repeat " %}

Image is from the paper: <a href = "https://www.nature.com/articles/s41576-019-0106-6">Regulation of transposable elements by DNA modifications </a>
<br>
<br>
<a href = "https://en.wikipedia.org/wiki/Long_interspersed_nuclear_element">LINE</a> stands for Long Interspersed Nuclear Elements. It conatains a group of non-LTR (long terminal repeat) retrotransposons that are widespread in the genome of many eukaryotes. There are five main groups - L1, RTE, R2, CR1 and Jockey.

  {% include figure image_path="assets/images/annotation/repetitive_elements/line_structure.png"  caption="Source - wikipedia: EN - endonuclease domain, RT - reverse transcriptase, IRES - internal ribosomal entry site, TSD - tandem site duplication, TSS - transcription start site " %}

All LINEs contain at least one open reading frame (ORF2) which codes for a reverse transcriptase (RT) and endonuclease (EN). Many also have a second open reading (ORF1) coding for an additional protein such as Gag-knuckle. The most common LINE in humans is LINE1

LINE elements are majority of most animal genome transposable elements

</details>
{: .notice--info}


**CRE/SLACS**: Count: 0  	Bases: 0 bp   	Percent: 0 %
{: .notice--info}

CRE/SLACS are not found outside Trypanosomatida unicellular organisms so they shouldn't be showing up in echinoderms or in other organisms. Papers can be found [here](https://www.tandfonline.com/doi/full/10.1080/2159256X.2016.1240747) and [here](https://pubmed.ncbi.nlm.nih.gov/26693517/) regarding their structure and presence. 




<details>
  <summary><b>L2/CR1/Rex</b> Count: 26022 &emsp; 	Bases: 9397503 bp &emsp; 	Percent: 1.49 %</summary>
  
  CR1 (chicken repeat 1), L2 (Line-2), and Rex transposable elements are considered sister classes to each (dfam). 

<a href="https://dfam.org/family/DF0000358/summary">L2 (LINE2)</a> spread before the mammalian radiation, and copies are only 65-75% similar to the consensus.  
  
{% include figure image_path="assets/images/annotation/repetitive_elements/rex_structure.png"  caption="Source - Figure  3 Carducci et al 2018" %}

Figure 3: Main features of the Rex1 retroelement (A/P*, apurinic/apyrimidinic site that can be located upstream or downstream the RT-encoding region; RT, reverse transcriptase-encoding region; 3′UTR), Rex3 (EN, endonuclease-encoding region; TSD, target site duplication; RT, reverse transcriptase-encoding region; Ct, C-terminal domain–encoding region; tail consisting of tandem repeats of the sequence GCG (GAA)2 (GATG)n, where n = 8–17, and Rex6 (RT, reverse transcriptase-encoding region; EN, endonuclease-encoding region). - taken from <a href="https://www.mdpi.com/1422-0067/19/11/3653/htm#B35-ijms-19-03653"> Rex Retroelements and Teleost Genomes: An Overview </a>  

Rex transposable elements were were discovered in the fish model Xiphophorus and are found throughout teleost genomes. They share many features with CR1 and L2 elements. 

<br>
<br>

{% include figure image_path="assets/images/annotation/repetitive_elements/CR1_structure.jpeg"  caption="Source - Figure 1 CR1 structure - Kapitonov and Jurka 2003" %}

Figure 1: Schematic structure of complete CR1-like retrotransposons from fishes and insects. CR1-1_DR, CR1-2_DR and CR1-3_DR—are consensus sequences of retrotransposons that belong to the three families of retrotransposons identified in the Danio rerio genome. Maui and Rex1 are the consensus sequences of two retrotransposons from CR1-like families present in the Fugu rubripes genome. CR1_OL is a slightly damaged element identified in the Oryzias latipes genome. Horizontally shaded boxes mark ORF1s and ORF2s. ORF2s encode proteins composed of the apurinic/apyrimidinic endonuclease (APE) and reverse transcriptase (RT) domains. Proteins encoded by ORF1s are composed of putative zinc finger/leucine zipper (ZL) motifs, the plant homeodomain (PHD) and the esterase (ES) domains. Black squares, diamonds and hexagons indicate different unclassified domains. The 3′ termini of all retrotransposons, excluding CR1-2_DR and CR1_DM, are shown starting from the polyadenylation signal, followed by terminal microsatellite repeats composed of different 4–7-bp units repeated 2–8 times. The average number of the repetitions is shown as a subscript index - <a href="https://academic.oup.com/mbe/article/20/1/38/1016286">The Esterase and PHD Domains in CR1-Like Non-LTR Retrotransposons</a>

CR1 is an abbreviation for Chicken-repeat 1. The original paper can be found <a href= "https://www.sciencedirect.com/science/article/abs/pii/S037811199700276X">here</a>. Like most LINE elements it is characterized by two open reading frames and is usually located near a TATA box. A 2005 paper -
<a href="https://genome.cshlp.org/content/15/1/126.full">The repetitive landscape of the chicken genome</a> - 

<br>


{% include figure image_path="assets/images/annotation/repetitive_elements/line2_structure.png"  caption="Source - Figure 1b Zebra Fish Line 2 structure - Sugano et al 2006" %}

(B) Schematic representation of ZfL2-1 and ZfL2-2. ZfL2-1 is
approximately 5.0 kbp long and has two ORFs, ORF1 and ORF2. ZfL2-1 ORF1 encodes a protein composed of a putative coiled-coil (CC) motif and the esterase (ES)
domain. The protein encoded by ZfL2-1 ORF2 is composed of an endonuclease (EN) domain and a reverse transcriptase (RT) domain. ZfL2-2 is approximately 4.2
kbp long and has one ORF. Horizontal arrowheads indicate repeat units of the repeat sequence in the 5′ untranslated region (UTR) of ZfL2-2. The protein encoded by
ZfL2-2 ORF is composed of EN and RT domains. The probe sequences used to isolate the full-length LINEs are indicated by black bars. - <a href="https://www.sciencedirect.com/science/article/abs/pii/S0378111905006542">Isolation and characterization of retrotransposition-competent LINEs from zebrafish</a>

<br>
<br>

L2, CR1 rex-baber - [Evolution of the Australian Lungfish (Neoceratodus forsteri) Genome: A Major Role for CR1 and L2 LINE Elements](https://academic.oup.com/mbe/article/29/11/3529/1153659)
  stuff about distribution, and structure
</details>
{: .notice--info}

LINEs make a 5% of the sea cucumber genome and L2/CR1/Rex contribute significantly to that number in other animal genomes. Due to echinoderma's relation to deuterostomes it shouldn't be a surprise that there are a number of L2/CR1/Rex elements in the the sea cucumber genome. 

<details>
  <summary><b>R1/LOA/Joecky</b>  	Count: 0 &emsp; 	Bases: 0 bp &emsp; 	Percent: 0 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/schematic_strucuture_site_specific_non-LTR.png"  caption="Source: Haruhiko Fujiwara 2015 - FIGURE 2 Schematic structure of each clade of site-specific non-LTR retrotransposons. The ORF1 in APE-encoding elements is shown as a short rectangle. An ORF2 in APE- encoding elements or ORF in RLE-encoding elements is shown as a long rectangle. A ZF-like structure is shown as a vertical bold line. A ZF-like structure in ORF1 of APE- encoding elements is shown as a zinc knuckle: Cx2Cx4Hx4Cx5-8Cx2Cx3Hx4C. In R2 clade elements, some elements have an additional ZF-like structure shown as a dotted vertical line. Myb-like (Myb) domains are found in R2 clade-elements and in TRAS families in the R1-clade. https://doi.org/10.1128/microbiolspec.MDNA3-0001-2014" %}
  <br>
  <br>
  <a href="https://onlinelibrary.wiley.com/doi/abs/10.1128/9781555817954.ch34">R2 and Related Site-Specific Non-Long Terminal Repeat Retrotransposons</a>
  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  "Originally the I group included five clades (I, Ingi, R1, LOA and Tad1) (Eickbush and Malik, 2002). The distinctive feature of this group is an RNase H that is downstream of RT, although many elements have lost the RNase H. It had been considered that the last common ancestor of the I group acquired an RNase H domain, but recent findings for RNase H domains from the Dualen, L1 and Proto1 families indicate that the acquisition of RNase H was an earlier event (Kojima and Fujiwara, 2005a; Kapitonov et al., 2009)"
  <br>
  "The I group has also been found in animals, fungi and trypanosomatids."
<br>
<br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  Originally the I group included five clades (I, Ingi, R1, LOA and Tad1) (Eickbush and Malik, 2002). The distinctive feature of this group is an RNase H that is downstream of RT, although many elements have lost the RNase H. It had been considered that the last common ancestor of the I group acquired an RNase H domain, but recent findings for RNase H domains from the Dualen, L1 and Proto1 families indicate that the acquisition of RNase H was an earlier event (Kojima and Fujiwara, 2005a; Kapitonov et al., 2009). The Jockey clade was considered a representative of the “Jockey group”; however, there is an accumulation of evidence that the Jockey clade is more closely related to the I group than the CR1 clade (Kojima and Fujiwara, 2005a; Putnam et al., 2007). Thus, here, the Jockey clade is proposed to be included in the I group,
</details>
{: .notice--info}

 <details>
  <summary><b>R2/R4/NeSL</b>  	Count: 0 &emsp; 	Bases: 0 bp &emsp; 	Percent: 0 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/schematic_strucuture_site_specific_non-LTR.png"  caption="Source: Haruhiko Fujiwara 2015 - FIGURE 2 Schematic structure of each clade of site-specific non-LTR retrotransposons. The ORF1 in APE-encoding elements is shown as a short rectangle. An ORF2 in APE- encoding elements or ORF in RLE-encoding elements is shown as a long rectangle. A ZF-like structure is shown as a vertical bold line. A ZF-like structure in ORF1 of APE- encoding elements is shown as a zinc knuckle: Cx2Cx4Hx4Cx5-8Cx2Cx3Hx4C. In R2 clade elements, some elements have an additional ZF-like structure shown as a dotted vertical line. Myb-like (Myb) domains are found in R2 clade-elements and in TRAS families in the R1-clade. https://doi.org/10.1128/microbiolspec.MDNA3-0001-2014" %}
 <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>

  "The R2 group is one of the five original groups (Eickbush and Malik, 2002). The R2 group includes the clades R2, R4, NeSL and Hero."
  <br>
  <br>
  The R2 group and CRE group share one fea- ture, namely that an RLE is encoded downstream of the RT. The other structures are not conserved through- out the group, although members often have zinc-finger motif(s) at the N-terminus of their encoded protein. Some families encode a Ulp1-type protease upstream of the RT. Some have two open reading frames (ORFs), while others have only one.

   <br>
  <a href="https://journals.asm.org/doi/abs/10.1128/microbiolspec.MDNA3-0011-2014">Integration, Regulation, and Long-Term Stability of R2 Retrotransposons</a>
  <br>

</details>
{: .notice--info}

<details>
  <summary><b>RTE/Bov-B</b> Count: 1890 &emsp; 	Bases: 838698 bp &emsp; 	Percent: 0.13 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>
  "The RTE group is also one of the five origi- nal groups (Eickbush and Malik, 2002). It originally included a single clade, RTE, but now includes several more (RTEX, RTETP and Proto2). The RTE group has been found in animals, fungi, plants and algae. How- ever, the distribution of clades, except for RTE and RTEX, are is quite restricted. The RTETP clade has been only found in diatoms."
  <br>
  <br>
  "Bov-B from the bovine Bos taurus, Expander from the pufferfish T. rubripes, and SR2 from the blood- fluke Schistosoma mansoni belong to this clade. Ele- ments belonging to the RTE clade are generally short and encode a protein with two functional domains: APE and RT."

  <br>
  <a href="https://link.springer.com/article/10.3103/S0891416816010043">Structure and distribution of the retrotransposon Bov-B LINE</a>
  <br>
  <br>
  <a href="https://www.sciencedirect.com/science/article/abs/pii/S0959437X17301089">The Retrotransposon storm and the dangers of a Collyer's genomeE</a>
  <br>
  <br>
</details>
{: .notice--info}

<details>
  <summary><b>L1/CIN4</b> Count: 0 &emsp; 	Bases: 0 bp &emsp; 	Percent: 0 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>
  "The L1 group is one of the five original groups (Eickbush and Malik, 2002)....Canonical elements that belong to the L1 group encode two proteins. The sequence of the ORF1 pro- tein is highly diverged: the ORF1 protein of human L1 has a leucine-zipper motif, while others have zinc-finger motifs. The second protein (ORF2) includes an APE, RT, and often a CCHC-type zinc finger motif. The L1 group does not encode an RLE, but some lineages of L1, especially L1 families from plants, encode an RNase H domain downstream of the RT domain. 
  <br>
  The L1 clade is represented by LINE1 (long inter- spersed element 1), found in various mammals. L1 is the only active autonomous non-LTR retrotransposon family in the human genome and causes cancers and genetic dis- eases by transposition."

</details>
{: .notice--info}

<details>
  <summary><b>LTR elements</b>  	Count: 28797 &emsp; 	Bases: 16024153 bp &emsp; 	Percent: 2.53 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}
  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>
  "LTR retrotransposons contain LTRs at both ends, and between these ends there are protein-coding regions. Proteins may contain several catalytic domains: protease, RT, ribonuclease H (RNase H) and integrase; there are also structural proteins called Gag and occasionally Env. LTR retrotransposons mobi- lize through reverse transcription of their own mRNA, catalyzed by RT. cDNA is generated as extrachromo- somal DNA and is then integrated into the genome by integrase. Integrase of LTR retrotransposons shows similarity to the transposase of some DNA transposons, especially the Ginger1 and Ginger2 superfamilies, which indicates the composite origin of LTR retrotransposons (Bao et al., 2010). LTR retrotransposons are subdivided into four superfamilies: Copia, Gypsy, BEL and endog- enous retroviruses (ERVs) (Table 2). The International Committee on Taxonomy of Viruses (ICTV) classifies some LTR retrotransposons as virus families."
</details>
{: .notice--info}

<details>
  <summary><b>BEL/Pao</b>  	Count: 1598 &emsp; 	Bases: 1246057 bp &emsp; 	Percent: 0.2 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>

 Belpaoviridae includes only one genus, Semotivirus, in the taxonomy of ICTV. Semoti- virus corresponds to the BEL superfamily of LTR ret- rotransposons. GyDB divided BEL into three branches and further into five clades (BEL, Tas, Suzu, Sinbad and Pao) (Llorens et al., 2011).

   <br>
  <a href="https://bmcecolevol.biomedcentral.com/articles/10.1186/1471-2148-11-154">BEL/Pao retrotransposons in metazoan genomes</a>
  <br>
  <br>

</details>
{: .notice--info}

<details>
  <summary><b>Ty1/Copia</b> Count: 122 &emsp; 	Bases: 169182 bp &emsp; 	Percent: 0.03 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>

One feature that distinguishes Copia from other LTR retrotransposons is the position of the integrase domain, which is upstream of the RT domain. With some exceptions, Gypsy, BEL and retrovi- ruses encode an integrase downstream of RT.

</details>
{: .notice--info}

 <details>
  <summary><b>Gypsy/DIRS1</b> Count: 13852 &emsp; 	Bases: 10059761 bp &emsp; 	Percent: 1.59 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>

The taxonomy of ICTV contains two genera under the family Metaviridae: Errantivirus and Metavirus. The representative family of Errantivi- rus is Gypsy from the fruit fly D. melanogaster. Metavi- rus corresponds to most of the Gypsy superfamily of LTR retrotransposons.

  <br>
  <a href="https://journals.asm.org/doi/full/10.1128/mbio.00187-22">A Missing Link between Retrotransposons and Retroviruses</a>
  <br>
  <br>
</details>
{: .notice--info}

<details>
  <summary><b>Retroviral</b>  	Count: 201 &emsp; 	Bases: 114432 bp &emsp; 	Percent: 0.02 %</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>
Retroviruses are a specialized branch inside LTR retrotransposons. Retroviruses generally have an envelope protein gene, env, in addition to other genes encoded in LTR retrotransposons. Env typically contains two domains: a transmembrane domain and a host receptor- binding domain. ERVs are retroviruses that omit the extracellular stage of their life cycle and replicate them- selves in germ cells. Some retain the coding ability for Env, but most do not. The loss of env and the expansion of ERVs by intracellular retrotransposition are strongly correlated (Magiorkinis et al., 2012).
  <br>
  <br>
  <a href="https://journals.asm.org/doi/full/10.1128/mbio.00187-22">A Missing Link between Retrotransposons and Retroviruses</a>
  <br>
  <br>
</details>
{: .notice--info}

### DNA transposons

[RepeatLandscape](https://github.com/caballero/RepeatLandscape) for creating the repeatmasker kimura distance graphs?
repeatmasker has a tool for generating the kimura distance plots: [createRepeatLandscape](https://github.com/rmhubley/RepeatMasker/blob/master/util/createRepeatLandscape.pl) called repeatlandscape

| DNA transposons  |                   | 40162 | 13472419 bp | 2.13 % |
|------------------|-------------------|-------|-------------|--------|
|                  | hobo-Activator    | 17290 | 4162041 bp  | 0.66 % |
|                  | Tc1-IS630-Pogo    | 534   | 120054 bp   | 0.02 % |
|                  | En-Spm            | 0     | 0 bp        | 0 %    |
|                  | MuDR-IS905        | 0     | 0 bp        | 0 %    |
|                  | PiggyBac          | 107   | 61946 bp    | 0.01 % |
|                  | Tourist/Harbinger | 7460  | 1910919 bp  | 0.3 %  |

<details>
  <summary><b>DNA transposons </b>  	Count: 40162 &emsp; 	Bases: 13472419bp &emsp; 	Percent: 2.13% </summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>

 The dominant group of DNA transposons is the TEs that encode DDD/E transposase as an enzyme for mobilization, both in eukaryotes and prokaryotes (Siguier et al., 2006; Bao et al., 2015). As of January 2018, Repbase contains 23 Class II TE super- families (Bao et al., 2015). Among them, 21 (Mariner/ Tc1, hAT, MuDR, EnSpm/CACTA, piggyBac, P, Merlin, Harbinger, Transib, Polinton, Kolobok, ISL2EU, Sola, Zator, Zisupton, Ginger1, Ginger2/TDD, Academ, Novosib, IS3EU and Dada) are known to encode DDD/E transposase for catalysis during integration.

Similarities between eukaryotic and prokaryotic DDD/E transposases are sometimes observed
</details>
{: .notice--info}

<details>
  <summary><b>hobo-Activator (hAT)</b>  	Count: 17290 &emsp; 	Bases: 4162041 bp &emsp; 	Percent: 0.66% </summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

  <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>

The hAT superfamily is one of the most abundant DNA transposon superfamilies.
Although no hAT DNA transposons are active in the human genome, many ancient hAT transposons, Charlie and its non-autonomous derivatives, preserve their traces on the human genome (Kojima, 2018a).
In general, hAT families encode a single protein that includes a transposase domain. TIRs of hAT families are usually short, up to 50 bp. The majority of hAT families generate 8-bp TSDs. However, hAT5 families generate 5-bp TSDs, hAT6 families generate 6-bp TSDs and hATw generates 7-bp TSDs. hATm and hATx are distinct lin- eages inside the hAT superfamily of DNA transposons.
</details>
{: .notice--info}

<details>
  <summary><b>Tc1-IS630-Pogo</b>  	Count: 534 &emsp; 	Bases: 120054bp &emsp; 	Percent: .02% </summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

 <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>
The eukaryotic superfamily Mariner/ Tc1 is related to the bacterial IS630 family (Doak et al., 1994). This group is also referred to as the IS630-Tc1- Mariner (ITm) family. Many subdivisions in the Mariner/ Tc1 superfamily have been proposed. These include Tc1, Mariner, Pogo, MaT, ITmD37D, ITmD37E, and so on (Shao and Tu, 2001; Claudianos et al., 2002; Coy and Tu, 2005; Tellier et al., 2015).

The centromeric protein CENP-B is a Pogo transposase that acquired a biological function (Tudor et al., 1992; Smit, 1996  
</details>
{: .notice--info}

<details>
  <summary><b>En-Spm</b>  	Count: 0 &emsp; 	Bases: 0bp &emsp; 	Percent: 0% </summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}
 <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>
Peterson characterized an autonomous TE insertion designated Enhancer (En) (Peterson, 1953). McClintock independently characterized a TE inser- tion and designated it as Suppressor-Mutator (Spm) (McClintock, 1954). These two TE insertions were sequenced and revealed to be almost identical, and we now refer to this family of TEs as Enhancer/Suppressor- Mutator (En/Spm). The EnSpm superfamily is also called the CACTA superfamily because many plant EnSpm family sequences begin with the pentanucleotide CACTA. EnSpm families usually encode two proteins and plant EnSpm families generate 3-bp TSDs.

</details>
{: .notice--info}

<details>
  <summary><b>MuDR-IS905</b>  	Count: 0 &emsp; 	Bases: 0bp &emsp; 	Percent: 0% </summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

The two-component system MuDR/Mu from maize comprises the first reported DNA transposon fam- ily that belongs to the superfamily currently recognized as MuDR or MULE (Mutator-like element) (Robertson, 1978). MuDR encodes two proteins, MURA and MURB; MURA is the transposase. MuDR families are primarily identified in plants, but have also been reported in ani- mals, fungi and stramenopiles.

Transposases of MuDR elements and prokaryotic IS256 elements share some features (Eisen et al., 1994; Hua-Van and Capy, 2008).

The N-terminus of MuDR contains a DNA-binding domain. The zinc-finger motif seen in the MuDRF fami- lies is called the GCM1 domain (Cantu et al., 2011).
</details>
{: .notice--info}

<details>
  <summary><b>PiggyBac</b>  	Count: 107 &emsp; 	Bases: 61946bp &emsp; 	Percent: .01% </summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

piggyBac was originally isolated from a baculovirus infecting a cell culture of the cabbage looper Trichoplusia ni (Fraser et al., 1983; Cary et al., 1989). The members of the piggyBac superfamily target a spe- cific sequence, TTAA. The transposases in the piggyBac superfamily have three conserved D residues and show similarity to that encoded by the bacterial IS4 family (Sarkar et al., 2003).
</details>
{: .notice--info}

<details>
  <summary><b>Tourist/Harbinger</b>  	Count: 7460 &emsp; 	Bases: 1910919bp &emsp; 	Percent: .4% </summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}

The Harbinger superfamily, or the PIF/ Harbinger superfamily, has two founder members, Harbinger and PIF (Jurka and Kapitonov, 2001). Harbinger was described from A. thaliana (Kapitonov and Jurka, 1999), while P instability factor (PIF) was characterized in maize (Zhang et al., 2001). PIF and related autonomous TEs are responsible for the mobiliza- tion of Tourist, which is one of the two predominant non- autonomous TE groups in plants. These TEs encode two proteins, ORF1 and transposase. The transposases show similarity to those encoded by IS5 and ISL2 in bacte- ria. The ORF1 protein usually contains a Myb-like DNA- binding domain and is required for transposition besides the transposase (Sinzelle et al., 2008). HarbingerS is a group of Harbinger families that encode three proteins: DDD/E transposase, SET histone methyltransferase and an unknown protein (Kojima and Jurka, 2014b)
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
  <summary><b>Other (Mirage, P-element, Transib</b>  	Count: 235 &emsp; 	Bases: 238763bp &emsp; 	Percent: .04%</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/"  caption="Source -" %}


The resemblance between V(D)J recombina- tion and the transposition of DNA transposons was rec- ognized just several years after V(D)J recombination was discovered (Sakano et al., 1979). The Transib super- family encodes a transposase that is most similar to the RAG1 protein, which is responsible for V(D)J recombina- tion (Kapitonov and Jurka, 2005). A protein similar to RAG2, another protein responsible for V(D)J recombina- tion, was identified in a lineage of Transib, TransibSU(Kapitonov and Koonin, 2015). A long-standing debate regarding the origin of V(D)J recombination was con- cluded upon the discovery of a Transib DNA transpo- son in the lancelet, designated ProtoRAG (Huang et al., 2016). ProtoRAG encodes two proteins that are similar to RAG1 and RAG2, and its termini resemble recombina- tion signal sequences.
<br>
<br>
The representative of the P superfamily, P element, was found in the genome of D. melanogaster (O’Hare and Rubin, 1983). The P superfamily is a relatively small group, despite the long research history: fewer than 200 families belonging to the P superfamily have been depos- ited in Repbase. The P superfamily is widely distrib- uted among animals, plants, fungi and protozoans. The human genome retains a catalytically active transposase of an ancient P family member as THAP9 (Majumdar et al., 2013).
</details>
{: .notice--info}

<details>
  <summary><b>Rolling-circles</b>  	Count: 1207 &emsp; 	Bases: 526201bp &emsp; 	Percent: .08%</summary>
  {% include figure image_path="assets/images/annotation/repetitive_elements/canoical_helitron_structure.png"  caption="Source: Thomas and Pritham 2015 - FIGURE 1 Structure and coding capacity of canonical animal and plant Helitrons, Helen- trons, Proto-Helentron, Helitron2 and IS91. (A) Structure of a typical animal Helitron. (B) A typical plant Helitron encoding Rep/Helicase and RPA proteins; one to three RPA genes can be found on either side of the Rep/Helicase gene. (C) Structure of a typical non- autonomous plant or animal Helitron; they do not encode the Rep/Helicase gene but share the common structural features. (D) Structure and coding capacity of a Helentron; Helentrons have sub terminal inverted repeats (subTIRs) (red), and a short palindrome at the 3′ end (stem loop). The subTIRs can either be palindromic or form a palindrome with the short inverted repeats (sideways triangle), near the subTIR, if present. (E) Structure of a Helentron-associated INterspersed Element (HINE); HINEs are nonautonomous but have the same structural features as that of the autonomous partner. (F) Structure and coding capacity of Proto-Helentron. (G) Structure and coding capacity of Helitron2 (re- drawn from reference 12). (H) Structure of a bacterial IS91 element that is proposed to transpose by rolling-circle mechanism (redrawn from references 9, 46). The genes that are occasionally carried by Helentrons are indicated with a black asterisk (*) and are in- cluded only if they were found in multiple families or across species. Sequences flanking the elements are shown in red. https://doi.org/10.1128/microbiolspec.MDNA3-0049-2014" %}

 <br>
  <a href="https://www.jstage.jst.go.jp/article/ggs/94/6/94_18-00024/_article/-char/ja/">Structural and sequence diversity of eukaryotic transposable elements</a>
  <br>
  <br>
Helitron is a unique group of DNA transposons in eukaryotes (Kapitonov and Jurka, 2001). Helitron usually encodes one protein, which includes two enzymatic domains: one is helicase and the other is the rolling-circle replication initiator (Rep). Rep is also called a “Y2 transposase”, because the conserved residues that are essential for transposition are two tyro- sines. Upstream of these Y2 motifs is a HUH motif, in which the U is any bulky hydrophobic residue. This HUH motif, as well as the conserved tyrosines, are known in other groups of mobile genetic elements, such as the IS91 and IS605 families of bacterial DNA transposons.

 <br>
  <a href="https://pubmed.ncbi.nlm.nih.gov/26350323/">Helitrons, the Eukaryotic Rolling-circle Transposable Elements </a>
  <br>
  <br>

</details>
{: .notice--info}

<details>
  <summary><b>Unclassified</b>  	Count: 571376 &emsp; 	Bases: 161042674bp &emsp; 	Percent: 25.47%</summary>
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