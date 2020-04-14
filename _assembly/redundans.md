---
title: "Redundans Reference Assisted Assembly"
toc: true
toc_sticky: true
layout: single

gallery:
  - url: /assets/images/redundans/scaffolds.reduced.fa.hist.png
    image_path: /assets/images/redundans/scaffolds.reduced.fa.hist.png
    title: "Identity between contigs"
  - url: /assets/images/redundans/contigs.reduced.fa.hist.png
    image_path: /assets/images/redundans/contigs.reduced.fa.hist.png
    title: "Identity between contigs"

---

## Redundans
Redundans is a pipeline that assists an assembly of heterozygous/polymorphic genomes. 

### Background
Excerpt from the github [page]()

Redundans pipeline assists an assembly of heterozygous genomes.
Program takes as input assembled contigs, sequencing libraries and/or reference sequence and returns scaffolded homozygous genome assembly. Final assembly should be less fragmented and with total size smaller than the input contigs. In addition, Redundans will automatically close the gaps resulting from genome assembly or scaffolding.

The pipeline consists of several steps (modules):

   * de novo contig assembly (optional if no contigs are given)
   * redundancy reduction: detection and selective removal of redundant contigs from an initial de novo assembly
   * scaffolding: joining of genome fragments using paired-end reads, mate-pairs, long reads and/or reference chromosomes
   * gap closing: filling the gaps after scaffolding using paired-end and/or mate-pair reads

Redundans is:

   * fast & lightweight, multi-core support and memory-optimised, so it can be run even on the laptop for small-to-medium size genomes
   * flexible toward many sequencing technologies (Illumina, 454, Sanger, PacBio & Nanopore) and library types (paired-end, mate pairs, fosmids, long reads)
   * modular: every step can be omitted or replaced by other tools
   * reliable: it has been already used to improve genome assemblies varying in size (several Mb to several Gb) and complexity (fungal, animal & plants)


### Installation
There was no conda package for redundans. I followed the github page installation instructions

```
git clone --recursive https://github.com/lpryszcz/redundans.git
cd redundans && bin/.compile.sh
```

bbduk contains scripts determining assembly stats. I installed it using conda. 
```
conda activate biotools
conda install -c bioconda bbmap 
```

conda package information
```
bbmap                     38.73                h516909a_0    bioconda
```

### Commands

running redundans
```
./redundans.py -i ../*.fq -t 40 -f ../Patanus-Allee/assembly/out_contig.fa --reference ../A_jap_genome.fasta --log cali-contig_v_jap.txt -o cali-contig_v_jap
```

stats script
```
stats.sh in=scaffolds.filled.fa out=pcali_redundans_stats.txt gchist=pcali_redundans_gc shist=pcali_redundans_len 
```

### Results

{% include gallery %}

stats results
```
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3140	0.1861	0.1862	0.3137	0.2528	0.0000	0.0000	0.3723	0.0513

Main genome scaffold total:         	304189
Main genome contig total:           	341022
Main genome scaffold sequence total:	878.550 MB
Main genome contig sequence total:  	656.423 MB  	25.283% gap
Main genome scaffold N/L50:         	908/232.994 KB
Main genome contig N/L50:           	15484/7.513 KB
Main genome scaffold N/L90:         	78509/985
Main genome contig N/L90:           	141289/686
Max scaffold length:                	2.355 MB
Max contig length:                  	342.646 KB
Number of scaffolds > 50 KB:        	2184
% main genome in scaffolds > 50 KB: 	67.80%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	       304,189	       341,022	   878,550,047	   656,422,679	  74.72%
    100 	       304,189	       341,022	   878,550,047	   656,422,679	  74.72%
    250 	       217,050	       253,883	   859,301,519	   637,174,151	  74.15%
    500 	       135,152	       171,982	   830,581,215	   608,458,355	  73.26%
   1 KB 	        77,404	       114,148	   789,640,472	   567,534,088	  71.87%
 2.5 KB 	        25,533	        62,147	   708,579,135	   486,496,492	  68.66%
   5 KB 	         8,199	        44,731	   649,692,467	   427,636,002	  65.82%
  10 KB 	         3,280	        39,741	   617,382,791	   395,449,317	  64.05%
  25 KB 	         2,450	        38,709	   606,035,901	   385,142,600	  63.55%
  50 KB 	         2,184	        37,710	   595,619,844	   379,278,395	  63.68%
 100 KB 	         1,663	        34,538	   558,268,199	   358,924,679	  64.29%
 250 KB 	           862	        25,459	   428,192,419	   283,136,250	  66.12%
 500 KB 	           328	        13,594	   237,332,936	   161,594,785	  68.09%
   1 MB 	            28	         2,024	    36,623,048	    28,079,209	  76.67%
```