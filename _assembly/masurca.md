---
title: "MaSuRCA Assembly"
toc: true
toc_sticky: true
layout: single
---

## MaSuRCA
Assemble the sea cuke genome using the MaSuRCA assembler

### Background

Excerpt from the MaSuRCA [website](http://www.genome.umd.edu/masurca.html)

MaSuRCA is whole genome assembly software. It combines the efficiency of the de Bruijn graph and Overlap-Layout-Consensus (OLC) approaches. MaSuRCA can assemble data sets containing only short reads from Illumina sequencing or a mixture of short reads and long reads (Sanger, 454, Pacbio and Nanopore).

More information can be found on their [github page](https://github.com/alekseyzimin/masurca)

### Installation

MaSuRCA was installed using conda

installation
```
 conda install -c bioconda masurca 
```

### Command   
Running MaSuRCA requires a bit more work then the normal assembler. First, MaSuRCA is run on config.txt file which has been previously edited to the desired parameters. The config file I used can be found [here](/assets/images/masurca/masurca_config.txt). This creates an assemble.sh file. Executing the assemble.sh file runs MaSuRCA.

```
./assemble.sh
```

### Results  
output was 1.4 terabytes of data, but no summary files. For results see Busco, Quast, and other analysis.