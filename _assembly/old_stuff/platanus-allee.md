---
title: "Platanus-Allee Assembly"
toc: true
toc_sticky: true
layout: single
permalink: /platanus-allee/
---

## Platanus-Allee
Assembly of hetereozygous genomes

### Background
Excerpt from the Platanus-Allee [website](http://platanus.bio.titech.ac.jp/platanus2)

We are pleased to announce that our novel genome assembler “Platanus-allee” has now been launched. Platanus-allee is an assembler derived from Platanus assembler, however, it was developed with another concept. Platanus-allee tries to construct each haplotype sequence from the beginning and pair them as homologous chromosomes, while Platanus constructs consensus sequence of homologous chromosomes at first and tries to split into each haplotype sequence. Therefore, Platanus-allee marks better performance for highly heterozygous species genome or highly diverged genomic regions. However, for low heterozygous species genome (as a guide < 1.0 %), Platanus assembler would mark better performance than Platanus-allee. This may be caused by the sequence coverage for the target sequence. Platanus-allee targets each haplotype, therefore, simply speaking, it requires a double number of reads.

### Installation
Platanus-Allee does not have a conda package. It requires downloading the prebuilt binary packages from their website. 

I downloaded v.2.0.2 Linux 64 bit binary (precompiled) and uncompressed it. 

### Command

```
./platanus_allee assemble -t 70 -m 352 -f ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

./platanus_allee consensus -t 70 -c out_primaryBubble.fa out_nonBubbleHomoCandidate.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 

./platanus_allee phase -t 70 -c out_contig.fa out_junctionKmer.fa -IP1 ../P_cali_g_1.fq.trimmed ../P_cali_g_2.fq.trimmed 
```

### Results

Resource usage for the contig assembly, consensus, and phasing steps respectively
```
#Assembly
#### PROCESS INFORMATION ####
VmPeak:         314.208 GByte
VmHWM:           21.938 GByte

#Phasing
#### PROCESS INFORMATION ####
VmPeak:          44.281 GByte
VmHWM:            2.374 GByte

#Consensus
#### PROCESS INFORMATION ####
VmPeak:           0.023 GByte
VmHWM:            0.005 GByte
```

Optimal kmer calculations
```
K=32, KMER_COVERAGE=89.9424 (>= 6), COVERAGE_CUTOFF=6
K=52, KMER_COVERAGE=73.8309, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-inf
K=72, KMER_COVERAGE=57.7195, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-inf
K=92, KMER_COVERAGE=41.608, COVERAGE_CUTOFF=6, PROB_SPLIT=10e-10.6771
K=107, KMER_COVERAGE=29.5244, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.1335
K=109, KMER_COVERAGE=27.9132, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.1843
K=110, KMER_COVERAGE=27.1077, COVERAGE_CUTOFF=2, PROB_SPLIT=10e-10.0229
```

Logs file can be found below   
[assemble.log](/assets/images/platanus-allee/assemble.log)   
[consensus.log](/assets/images/platanus-allee/consensus.log)   
[phase.log](/assets/images/platanus-allee/phase.log)   