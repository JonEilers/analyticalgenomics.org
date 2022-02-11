---

title: "Genome Assembly Quality Assessment using Inspector"
permalink: /inspector/
layout: single
toc: true 
toc_sticky: true

excerpt: "Good day Sir, I am here to inspect your assembly."

---

# Introduction

There is a bad habit in the genome assembly world of not checking the quality of your assembly before publishing it. Part of that is due to a lack of tools. [Inspector](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02527-4) is one of several solutions to this problem. Inspector uses long reads aligned to the genome to check quality. 

# The Assemblies

Three *Chiridota heheva* assemblies previously created using the assemblers Shasta, Flye, and Raven will be used for comparison. Note that these assemblies used long read nanopore data that was not error corrected. Additionally, the assemblies have not been polished. 

# Installation

Installation is fairly straight forward. Further instructions can be found at the Inspector [github](https://github.com/ChongLab/Inspector)

```bash
conda create -n inspector
conda activate inspector
conda install -c bioconda inspector
```

# Running Inspector

Running inspector requires a fasta/q file of long reads and a genome assembly to align the reads too. Additionally you can set an output directory and even add a reference genome to compare to. 

```bash

# Raven assembly
inspector.py \
    -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/raven/raven_c_heheva.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
    -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/raven \
    --datatype nanopore \
    -t 40 

# Shasta assembly
inspector.py \
    -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta/Assembly.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
    -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/shasta \
    --datatype nanopore \
    -t 40 

# Flye assembly
inspector.py \
    -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye/assembly.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
    -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/flye \
    --datatype nanopore \
    -t 40 
```

Note: Minimap uses a lot of power and will max out the UPS at 70 cpus (because m UPS doesn't provide as much power as the server can use. Price of being cheap.). This might be a python specific use of minimap. I do need to get a larger ups at some point

# Results

Inspector outputs a text document containing a summary of the results. Copying and pasting into excel and then into [excel to markdown table converter website](https://tableconvert.com/excel-to-markdown) generates the below table. 

| Statics of contigs:                | Raven Assembly | Shasta Assembly | Flye Assembly |
|------------------------------------|----------------|-----------------|---------------|
| Number of contigs                  | 3227           | 15199           | 16728         |
| Number of contigs > 10000 bp       | 3223           | 11941           | 13396         |
| Number of contigs >1000000 bp      | 207            | 28              | 179           |
| Total length                       | 1221957363     | 1209883028      | 1540823993    |
| Total length of contigs > 10000 bp | 1221922362     | 1196342865      | 1521394030    |
| Total length of contigs >1000000bp | 290685476      | 36833854        | 267358448     |
| Longest contig                     | 2901347        | 1748484         | 3605186       |
| Second longest contig length       | 2793378        | 1705511         | 3518942       |
| N50                                | 606319         | 201222          | 307072        |
| N50 of contigs >1Mbp               | 606319         | 201222          | 307072        |

yada yada 


| Read to Contig alignment:           | Raven Assembly | Shasta Assembly | Flye Assembly |
|-------------------------------------|----------------|-----------------|---------------|
| Mapping rate /%                     | 98.43          | 98.38           | 98.75         |
| Split-read rate /%                  | 27.4           | 30.49           | 21            |
| Depth                               | 34.5491        | 35.2633         | 27.7466       |
| Mapping rate in large contigs /%    | 21.93          | 2.89            | 18.42         |
| Split-read rate in large contigs /% | 23.79          | 20.73           | 16.53         |
| Depth in large conigs               | 32.8825        | 34.2379         | 29.7275       |

yada yada


| Error type       | Raven Assembly | Shasta Assembly | Flye Assembly |
|------------------|----------------|-----------------|---------------|
| Structural error | 4587           | 347             | 729           |
| Expansion        | 379            | 220             | 38            |
| Collapse         | 288            | 48              | 4             |
| Haplotype switch | 3920           | 79              | 686           |
| Inversion        | 0              | 0               | 1             |

yada yada

| Indels                              | Raven Assembly   | Shasta Assembly  | Flye Assembly    |
|-------------------------------------|------------------|------------------|------------------|
| Small-scale assembly error /per Mbp | 2161.13566796317 | 6993.43411054656 | 188.174131326123 |
| Total small-scale assembly error    | 2640740          | 8366545          | 286287           |
| Base substitution                   | 1240424          | 2714250          | 134786           |
| Small-scale expansion               | 818823           | 4165783          | 103520           |
| Small-scale collapse                | 581493           | 1486512          | 47981            |
|                                     |                  |                  |                  |
| QV                                  | 25.2858950332793 | 21.3835831443883 | 34.6460926406124 |

yada yada