---
title: "Identifying and Masking Repetitive Elements in the Genome Assembly"
toc: true
toc_sticky: true
layout: single
permalink: /repeats/

excerpt: “The genome is a book that wrote itself, continually adding, deleting and amending over four billion years.” ~ Matt Ridley


---

# Introduction

https://www.jstage.jst.go.jp/article/ggs/advpub/0/advpub_18-00024/_pdf/-char/ja
Class 2 transposable elements

[Helitron](https://en.wikipedia.org/wiki/Helitron_(biology))

[Polinton](https://en.wikipedia.org/wiki/Polinton) - largest and most complex 


- eukaryotic TEs are divided into two classes: Class I and Class II
- Class I includes retrotransposons, which transpose through an RNA inter-
mediate
- Class I is subdivided into two large categories that are distinguished by the presence of long terminal repeats (LTRs): LTR retrotransposons and non-LTR retrotransposons.
- Class II includes DNA transposons, which do not use RNA as
transposition intermediates

**What we should expect to see**
https://www.mdpi.com/1422-0067/22/4/2072/htm
(LINEs: 6–8 kb unit length) and ‘short interspersed nuclear elements’ (SINEs: 0.1–0.4 kb unit length). Furthermore, there are long terminal repeats that account for 8.3% of human genomes (0.2–3 kb unit length) [34].

    LINEs are formed by a group of mostly truncated retrotransposons and constitute >20% of the human genome. Three types of LINEs have been identified: LINE1 (~516,000 copies), LINE2 (~315,000 copies) and LINE3 (~37,000 copies). In fact, in humans, there are ~100 active LINEs per genome, which can still amplify and integrate at new genomic sites, as they comprise reverse transcriptase [26,27,28].
    Furthermore, SINEs derived from another subclass of retrotransposons provide ~13% of the human genome, with the feature that they can normally only become transcriptionally active if induced during infection of the human carrier by 
    multiple DNA viruses or supported by LINE1 elements [35,36,37,38].

 Accordingly, satellite DNAs (including classes 2 and 4) make up about 8–10% of the human genome; e.g., α-satellites are annotated at 44,058 loci covering 0.1% of the genome [42]. Interestingly, although these α- and γ-satellite sequences have been cloned, sequenced and known for decades, the majority of them are not included in genome browsers. Their sizes are variable between individuals; however, the ranges of the regions that they span have been previously reported to be between ~0.1 and 5 Mb [1]. At least some of these satellite DNAs are transcribed into RNA, but their role is yet unresolved. The fact that α-satellite DNA, for example, is expressed under cellular stress supports the idea that the alteration of heterochromatic to transcriptionally active regions could be correlated with genomic instability and oncogenesis; further supporting this notion is the fact that the tumor suppressor PKNOX1 inhibits such satellite expression. Thus, histone methylation is important for satellite DNA expression, too. As the methylation status is also altered by heat shock treatment, it is not surprising that α-satellite sequences in chromosomes 12 and 15 were proven to be expressed after a heat shock in 2004 [1]. 

# Installation

```bash
# installing EDTA
conda create -n edta
conda activate edta
conda install -c bioconda edta 
```

installing [TETools](https://github.com/Dfam-consortium/TETools)
```bash
# downloads the scripts
curl -sSLO https://github.com/Dfam-consortium/TETools/raw/master/dfam-tetools.sh

# makes the script executable
chmod +x dfam-tetools.sh

# creating conda environment and installing singularity 
conda create -n singularity singularity
conda activate singularity

# runs the script which pulls the container image and runs it
./dfam-tetools.sh 
```

It is preferred to run this container using singularity as that will prevent the headache of mounting folders and other nonsense that docker makes you do. Unfortunately I wasn't able to get singularity working with conda and I didn't feel like going through the hasle of installing all the dependencies and compiling it so I ended up using Docker. 

# Downloading Database

# Running Tetools

When using docker to run TETools it mounts the current working directory. So if you will be pulling files from numerous directories you can go to the highest level and run it from there if you don't want to mount abunch of directories. 

```bash
# this sets up the singularity container and allows you to run stuff inside it
sudo ./dfam-tetools.sh

# once in use the below commands
BuildDatabase \
    -name ajapmasurca \
    /work/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta

RepeatModeler -database database/ajapmasurca -LTRStruct -pa 8


# realizing I may have used the wrong assembly for running repeatmodeler. Going to rerun using the assembly that EDTA renamed.

BuildDatabase \
    -name edta_ajapmasurca \
    /work/edta/primary.genome.scf.fasta.mod


RepeatModeler -database edta_database/edta_ajapmasurca -LTRStruct -pa 8

# nevermind. EDTA only needed the repeat library which doesn't have scaffold/contig names

RepeatMasker genome1.fa [-lib library.fa] -pa 8

RepeatMasker /work/edta/primary.genome.scf.fasta.mod -species Deuterostomia -xsmall -html -source -gff -pa 8


runcoseg.pl -d -m 50 -c ALU.cons -s ALU.seqs -i ALU.ins

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

https://www.clementgoubert.com/post/a-simple-pipeline-for-te-annotation-in-an-assembled-genome


https://www.biostars.org/p/411101/

https://github.com/Dfam-consortium/RepeatModeler/issues/40

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