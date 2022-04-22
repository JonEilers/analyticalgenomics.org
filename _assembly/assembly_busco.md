---
title: "Checking genome assembly quality using BUSCO"
toc: true
toc_sticky: true
layout: single
permalink: /busco/

excerpt: "Sir, I think we've found them"

gallery:
  - url: /assets/images/assembly/busco/busco_analysis.png
    image_path: /assets/images/assembly/busco/busco_analysis.png
    title: "Comparison of buscos between genome assemblies"
    caption: 

---

# Introduction
In 2015, a group of scientists published a paper (and tool) about "Benchmarking Universal Single-Copy Orthologs". They called their tool Busco. Which coincidentally enough means "I search" in spanish. They searched through a large number of genomes, identified genes that are single copy orthologs, and then used this database to search newly published genomes as a way to assess the genome assembly quality.  

Let's break this down more - [orthologous genes](https://en.wikipedia.org/wiki/Sequence_homology#Orthology) are genes which share a common ancestry and are a product of speciation. More specifically, the gene sequences diverged through mutations but both originated from the same sequence. This is different from gene duplication events which results in gene [paralogs](https://pubmed.ncbi.nlm.nih.gov/16285863/).

BUSCO only uses genes that are single copy in most genomes. This reduces the risk of a false positive or paralogs being confused for orthologs. Also important is that if a gene is universially found to be single copy, this implies there are some biological/evolution contraints on that genes. Most genomes have undergone duplication events at one time or another. This means that at one point there were multiple copies, but through time all but one copy was lost and this has happened across all organisms. The implications are that these genes are likely to be found in all organisms and could be used as a benchmark of genome assembly quality. 

{% include figure image_path="assets/images/assembly/busco/busco_sampling.png"  caption="Busco distribution. I am honestly not sure how to interpret this graph from the authors website, but it looks significant and interesting. It showing something about how there are single copy genes found in a majority of species." %}

Thus BUSCO has become a ubiqitous tool in genome assembly evaluation. It is to the point where I have heard scientists smuggly throw the term BUSCO around to show they are genome bioinformaticians and are in the "know" as genome assembly is still something of a niche field of bioinformatics. 

While I will be going through how to use the tool and interpret the results, the authors have an excellent [website](https://busco.ezlab.org/) and detailed [user guide](https://busco.ezlab.org/busco_userguide.html) that I recommend you take a look at. 


# BUSCO and MultiQC installation
BUSCO was installed using conda

```bash
conda create -n busco
conda activate busco
conda install -c bioconda busco 
```

Sometimes conda doesn't want to install due to strange reasons. If you run into this, the authors have also created and maintain a docker image. 

If you are running busco across many assemblies, there is a handy tool for compiling the results into one figure. [MultiQC](https://multiqc.info/) is a tool for merging multiple results from a single tool into one figure. It has a module specifically for busco that I will use below. 

```bash
conda create -n multiqc
conda activate multiqc
conda install -c bioconda multiqc 
```

# Running BUSCO analysis

I ran Busco awhile back on three assemblies I generated for the *Apostichopus californicus* genome. The first two used Masurca and platanus-allee which are de-novo hybrid genome assemblers that can be used with short read data. The third assembly was created using redundans and the best sea cucumber genome assembly available at the time as a reference. The short read assemblies both have an N50 less than 10kbp and contain hundreds of thousands of scaffolds/contigs so they are low quality draft assemblies. The redundans assembly is significantly better, but because it used a different species as a reference the assembly content is quite suspect. 

```bash
# For Eukaryote Buscos
# Platanus-allee
busco -c 5 -m genome -i ../out_consensusScaffold.fa -o busco_out_consensusScaffold.fa -l eukaryota_odb10

# MaSuRCA
busco -c 40 -m genome -i ../masurca_p_cali.fasta -o euk_busco_masurca_pcali -l eukaryota_odb10

# Redundans
busco -c 30 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o euk_busco_pcali_ajap.redundans.platanus.fa -l eukaryota_odb10


# For Metazoa Buscos
# Platanus-allee
busco -c 5 -m genome -i ../out_consensusScaffold.fa -o metazoa_busco_out_consensusScaffold.fa -l metazoa_odb10

# MaSuRCA
busco -c 40 -m genome -i ../masurca_p_cali.fasta -o metazoa_busco_masurca_pcali -l metazoa_odb10

# Redundans
busco -c 30 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o metazoa_busco_pcali_ajap.redundans.platanus.fa -l metazoa_odb10
```

Additionally, I ran busco on the published *Apostichopus japonicus* genome assembly and two assemblies generated by Masurca - a low and high confidence one. 
```bash
 busco \
 	-i /Users/jon/Documents/genomes/Ajap_genome.fasta \
	-l eukaryota_odb10 \
	-o ajap_euk \
	-m genome \
	-c 3

 busco \
 	-i /Users/jon/Documents/genomes/scaffolds.ref.fa \
	-l eukaryota_odb10 \
	-o ajap_masurca_scaffolds_euk \
	-m genome \
	-c 3

 busco \
 	-i /Users/jon/Documents/genomes/primary.genome.scf.fasta \
	-l eukaryota_odb10 \
	-o ajap_masurca_euk \
	-m genome \
	-c 3

 busco \
 	-i /Users/jon/Documents/genomes/Ajap_genome.fasta \
	-l metazoa_odb10 \
	-o ajap_met \
	-m genome \
	-c 3

 busco \
 	-i /Users/jon/Documents/genomes/scaffolds.ref.fa \
	-l metazoa_odb10 \
	-o ajap_masurca_scaffolds_met \
	-m genome \
	-c 3

 busco \
 	-i /Users/jon/Documents/genomes/primary.genome.scf.fasta \
	-l metazoa_odb10 \
	-o ajap_masurca_met \
	-m genome \
	-c 3
```

There are no busco databases available specifically for echinoderms. So the next best options are metazoa and eukaryota. I like to run both as the genes used are different between the two datasets. 

# Results

Looks like the Masurca assembly may contain a lot of duplicated buscos, but also had more fragmented and missing buscos.

## Figures

{% include gallery %}

 
**Platanus-Allee**
Eukaryote
```
# BUSCO version is: 4.0.2 
# The lineage dataset is: eukaryota_odb10 (Creation date: 2019-11-20, number of species: 70, number of BUSCOs: 255)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/out_consensusScaffold.fa
# BUSCO was run in mode: genome

	***** Results: *****

	C:78.4%[S:75.7%,D:2.7%],F:16.1%,M:5.5%,n:255	   
	200	Complete BUSCOs (C)			   
	193	Complete and single-copy BUSCOs (S)	   
	7	Complete and duplicated BUSCOs (D)	   
	41	Fragmented BUSCOs (F)			   
	14	Missing BUSCOs (M)			   
	255	Total BUSCO groups searched
```

Metazoa
```
# The lineage dataset is: metazoa_odb10 (Creation date: 2019-11-20, number of species: 65, number of BUSCOs: 954)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/out_consensusScaffold.fa
# BUSCO was run in mode: genome

	***** Results: *****

	C:82.5%[S:78.6%,D:3.9%],F:12.3%,M:5.2%,n:954	   
	787	Complete BUSCOs (C)			   
	750	Complete and single-copy BUSCOs (S)	   
	37	Complete and duplicated BUSCOs (D)	   
	117	Fragmented BUSCOs (F)			   
	50	Missing BUSCOs (M)			   
	954	Total BUSCO groups searched
```

**Masurca**
Eukaryote
```
# BUSCO version is: 4.0.2 
# The lineage dataset is: eukaryota_odb10 (Creation date: 2019-11-20, number of species: 70, number of BUSCOs: 255)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/masurca_p_cali.fasta
# BUSCO was run in mode: genome

	***** Results: *****

	C:69.8%[S:54.1%,D:15.7%],F:20.0%,M:10.2%,n:255	   
	178	Complete BUSCOs (C)			   
	138	Complete and single-copy BUSCOs (S)	   
	40	Complete and duplicated BUSCOs (D)	   
	51	Fragmented BUSCOs (F)			   
	26	Missing BUSCOs (M)			   
	255	Total BUSCO groups searched
```

Metazoa 
```
# The lineage dataset is: metazoa_odb10 (Creation date: 2019-11-20, number of species: 65, number of BUSCOs: 954)
# Summarized benchmarking in BUSCO notation for file /home/jon/Working_Files/masurca_p_cali.fasta
# BUSCO was run in mode: genome

	***** Results: *****

	C:76.4%[S:61.6%,D:14.8%],F:13.6%,M:10.0%,n:954	   
	729	Complete BUSCOs (C)			   
	588	Complete and single-copy BUSCOs (S)	   
	141	Complete and duplicated BUSCOs (D)	   
	130	Fragmented BUSCOs (F)			   
	95	Missing BUSCOs (M)			   
	954	Total BUSCO groups searched
```

**Redundans**

Eukaryote
```
	--------------------------------------------------
	|Results from dataset eukaryota_odb10             |
	--------------------------------------------------
	|C:78.4%[S:73.3%,D:5.1%],F:12.5%,M:9.1%,n:255     |
	|200	Complete BUSCOs (C)                       |
	|187	Complete and single-copy BUSCOs (S)       |
	|13	Complete and duplicated BUSCOs (D)        |
	|32	Fragmented BUSCOs (F)                     |
	|23	Missing BUSCOs (M)                        |
	|255	Total BUSCO groups searched               |
	--------------------------------------------------
```

Metazoa
```
	--------------------------------------------------
	|Results from dataset metazoa_odb10               |
	--------------------------------------------------
	|C:84.2%[S:75.6%,D:8.6%],F:9.2%,M:6.6%,n:954      |
	|803	Complete BUSCOs (C)                       |
	|721	Complete and single-copy BUSCOs (S)       |
	|82	Complete and duplicated BUSCOs (D)        |
	|88	Fragmented BUSCOs (F)                     |
	|63	Missing BUSCOs (M)                        |
	|954	Total BUSCO groups searched               |
	--------------------------------------------------
```
