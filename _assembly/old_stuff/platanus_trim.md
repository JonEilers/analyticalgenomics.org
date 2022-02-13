---
title: "Platanus_trim"
toc: true
toc_sticky: true
layout: single
   
---

## Platanus_trim
Read quality filtering and adapter trimming

### Background
Excerpt from Platanus_trim [website](http://platanus.bio.titech.ac.jp/pltanus_trim)

The pre-processing tools for genome assembling, called Platnaus_trim and Platanus_internal_trim are now available. Platanus_trim is a tool for trimming adaptor sequences and low quality regions. In contrast, Platanus_internal_trim is a tool for trimming internal adaptor sequence, adaptor sequences, and low quality regions. Platanus_trim is designed for paired-end library and Platanus_internal_trim is for mate-pair library.

### Installation
There is no conda package for Platanus_trim. I downloaded the pre-compiled binary from the website: Platanus_trim : Linux 64 bit binary (precompiled)

### Commands
Adapters were provided by novogene in the report they provided with the raw sequence data. 

Adapters
```
Adapter sequences :
5' Adapter
5'-AATGATACGGCGACCACCGAGATCTACACTCTTTCCCTACACGACGCTCTTCCGATCT-3'

3' Adapter (Index is ATCACG found in the sequence)
5'-GATCGGAAGAGCACACGTCTGAACTCCAGTCACATCACGATCTCGTATGCCGTCTTCTGCTTG-3'
```

Command
```
./platanus_trim -i file_list -t 70 -1 AATGATACGGCGACCACCGAGATCTACACTCTTTCCCTACACGACGCTCTTCCGATCT -2 GATCGGAAGAGCACACGTCTGAACTCCAGTCACATCACGATCTCGTATGCCGTCTTCTGCTTG
```

### Results

Log output 
```
Running with trim adapter mode
Checking files: 
  P_cali_g_1.fq P_cali_g_2.fq  (100%)

Number of trimmed read with adapter: 
NUM_OF_TRIMMED_READ(FORWARD) = 106842360
NUM_OF_TRIMMED_BASE(FORWARD) = 6765543
NUM_OF_TRIMMED_READ(REVERSE) = 106840567
NUM_OF_TRIMMED_BASE(REVERSE) = 25360662
NUM_OF_TRIMMED_PAIR(OR) = 106843206
NUM_OF_TRIMMED_PAIR(AND) = 106839721


Number of trimmed read because of low quality or too short (< 11bp): 
NUM_OF_TRIMMED_READ(FORWARD) = 94401121
NUM_OF_TRIMMED_BASE(FORWARD) = 2779760352
NUM_OF_TRIMMED_READ(REVERSE) = 125115227
NUM_OF_TRIMMED_BASE(REVERSE) = 4115392502
NUM_OF_TRIMMED_PAIR(OR) = 160206782
NUM_OF_TRIMMED_PAIR(AND) = 59309566


#### PROCESS INFORMATION ####
User Time:         672.42 min
System Time:      1914.34 min
VmPeak:           5.233 GByte
VmHWM:            0.224 GByte
Execution time:    130.90 min
```