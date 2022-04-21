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

## Busco analysis of genome assemblies
Checking for conserved genes

### Background
Excerpt from the Busco [website](https://busco.ezlab.org/)

Based on evolutionarily-informed expectations of gene content of near-universal single-copy orthologs, BUSCO metric is complementary to technical metrics like N50.

### Installation
BUSCO was installed using conda

```
 conda install -c bioconda busco 
```

Conda package information
```
busco                     4.0.2                   pyr36_0    bioconda
```

### Commands

For Eukaryote Buscos
```
Platanus-allee
busco -c 5 -m genome -i ../out_consensusScaffold.fa -o busco_out_consensusScaffold.fa -l eukaryota_odb10

MaSuRCA
busco -c 40 -m genome -i ../masurca_p_cali.fasta -o euk_busco_masurca_pcali -l eukaryota_odb10

Redundans
busco -c 30 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o euk_busco_pcali_ajap.redundans.platanus.fa -l eukaryota_odb10
```

For Metazoa Buscos
```
Platanus-allee
busco -c 5 -m genome -i ../out_consensusScaffold.fa -o metazoa_busco_out_consensusScaffold.fa -l metazoa_odb10

MaSuRCA
busco -c 40 -m genome -i ../masurca_p_cali.fasta -o metazoa_busco_masurca_pcali -l metazoa_odb10

Redundans
busco -c 30 -m genome -i ../pcali_ajap.redundans.platanus.fasta -o metazoa_busco_pcali_ajap.redundans.platanus.fa -l metazoa_odb10
```

### Results

Looks like the Masurca assembly may contain a lot of duplicated buscos, but also had more fragmented and missing buscos.

#### Figures

{% include gallery %}

#### Platanus-Allee

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

#### Masurca 

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

#### Redundans

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



# to do

April 2022 - the latest conda busco version is being a butt and not installing. So I may have to redo this section using docker. ugh.  

https://busco.ezlab.org/busco_userguide.html#docker-image


