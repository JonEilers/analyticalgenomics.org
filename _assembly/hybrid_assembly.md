---

title: "Hybrid assembly using Platanus-allee and MaSURCA"
permalink: /hybrid_genome_assembly/
layout: single
toc: true 
toc_sticky: true

sidebar:
  nav: sidebar-main
header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "The Short and Long"

---


# Introduction
two genome assemblies of apostichopus japonicus using data from ncbi - not the original data for the published genome. 

platanus-allee assembly
masurca assembly

compare time, resources, and raw stats. 

have links to other comparisons such as mummer4 alignment to the original genome assembly, busco stats, blobtoolkit results. etc

# The Data

The current refseq *Apostichopus japonicus* genome assembly was published in 2017. The authors uploaded the gene expression data they used, but not the data used for the genome assembly. The only other genome assembly dataset on NCBI was published a year later, but oddly they never uploaded their genome assembly.

180bp - SRR6251237  
350bp paired end - SRR6255867  
500bp paired end - SRR6255868  
dunno? - SRR6255869  
450bp paired end - SRR6255870  
5kb long insert mate-pair - SRR6257769  
10kb long insert mate-pair - SRR6257771  
15kb long insert mate-pair - SRR6257773  
20kb long-insert mate-pair  - SRR6257777  
pacbio - SRR6282347  

# Hybrid Genome Assemblers

blah blah about hybrid genome assemblers

## Platanus-allee assembler

Started platanus-allee assemble: 14 Oct 2021
Started platanus-allee phase: 19 Oct 2021 
Started platanus-allee consensus: 02 Nov 2021

```bash
./platanus_allee assemble \
	-f /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_2.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_2.fastq \
	-o ajap_step10 \
	-s 10 \
	-t 75 \
	-m 360 2>assemble.log
```

```bash
./platanus_allee phase \
	-o ajap_step10 \
	-c /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/ajap_step10_contig.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
	-IP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_2.fastq \
	-IP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_2.fastq \
	-OP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_2.fastq \
	-OP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_2.fastq \
	-OP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_2.fastq \
	-OP4 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_2.fastq \
	-p /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
	-mapper /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/minimap2-2.0-r191/minimap2 \
	-t 70 2>phase.log
```


```bash
./platanus_allee consensus \
	-o ajap_step10_consensus \
	-c /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/ajap_step10_consensusInput.fa \
	-IP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
	-IP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_2.fastq \
	-IP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_2.fastq \
	-OP1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_2.fastq \
	-OP2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_2.fastq \
	-OP3 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_2.fastq \
	-OP4 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_1.fastq \
	/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_2.fastq \
	-p /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
	-mapper /home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64m/inimap2-2.0-r191/minimap2 \
	-t 70 2>consensus.log
```


## MaSURCA assembler

```bash
# example configuration file 

# DATA is specified as type {PE,JUMP,OTHER,PACBIO} and 5 fields:
# 1)two_letter_prefix 2)mean 3)stdev 4)fastq(.gz)_fwd_reads
# 5)fastq(.gz)_rev_reads. The PE reads are always assumed to be
# innies, i.e. --->.<---, and JUMP are assumed to be outties
# <---.--->. If there are any jump libraries that are innies, such as
# longjump, specify them as JUMP and specify NEGATIVE mean. Reverse reads
# are optional for PE libraries and mandatory for JUMP libraries. Any
# OTHER sequence data (454, Sanger, Ion torrent, etc) must be first
# converted into Celera Assembler compatible .frg files (see
# http://wgs-assembler.sourceforge.com)
DATA
PE=pq 180 27 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6251237_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6251237_2.fastq
PE=pw 350 53 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq
PE=pe 500 75 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255868_2.fastq
PE=pr 450 68 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255870_2.fastq
JUMP = mq 5000 750 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257769_2.fastq
JUMP = mw 10000 1500 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257771_2.fastq
JUMP = me 15000 2250 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257773_2.fastq
JUMP = mr 20000 3000 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_1.fastq /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6257777_2.fastq

PACBIO=/home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fasta


#Illumina paired end reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#if single-end, do not specify <reverse_reads>
#If mean/stdev are unknown use 500 and 50 -- these are safe values that will work for most runs
#MUST HAVE Illumina paired end reads to use MaSuRCA
#PE= pe 500 50  /FULL_PATH/frag_1.fastq  /FULL_PATH/frag_2.fastq
#Illumina mate pair reads supplied as <two-character prefix> <fragment mean> <fragment stdev> <forward_reads> <reverse_reads>
#JUMP= sh 3600 200  /FULL_PATH/short_1.fastq  /FULL_PATH/short_2.fastq
#pacbio OR nanopore reads must be in a single fasta or fastq file with absolute path, can be gzipped
#if you have both types of reads supply them both as NANOPORE type
#PACBIO=/FULL_PATH/pacbio.fa
#NANOPORE=/FULL_PATH/nanopore.fa
#Legacy reads (Sanger, 454, etc) in one frg file, concatenate your frg files into one if you have many
#OTHER=/FULL_PATH/file.frg
#synteny-assisted assembly, concatenate all reference genomes into one reference.fa; works for Illumina-only data
#REFERENCE=/FULL_PATH/nanopore.fa
END

PARAMETERS
#PLEASE READ all comments to essential parameters below, and set the parameters according to your project
#set this to 1 if your Illumina mate pair (jumping) library reads are shorter than 100bp
EXTEND_JUMP_READS=0
#this is k-mer size for deBruijn graph values between 25 and 127 are supported, auto will compute the optimal size based on the read data and GC content
GRAPH_KMER_SIZE = auto
#set this to 1 for all Illumina-only assemblies
#set this to 0 if you have more than 15x coverage by long reads (Pacbio or Nanopore) or any other long reads/mate pairs (Illumina MP, Sanger, 454, etc)
USE_LINKING_MATES = 0
#specifies whether to run the assembly on the grid
USE_GRID=0
#specifies grid engine to use SGE or SLURM
GRID_ENGINE=SGE
#specifies queue (for SGE) or partition (for SLURM) to use when running on the grid MANDATORY
GRID_QUEUE=all.q
#batch size in the amount of long read sequence for each batch on the grid
GRID_BATCH_SIZE=500000000
#use at most this much coverage by the longest Pacbio or Nanopore reads, discard the rest of the reads
#can increase this to 30 or 35 if your long reads reads have N50<7000bp
LHE_COVERAGE=25
#this parameter is useful if you have too many Illumina jumping library reads. Typically set it to 60 for bacteria and 300 for the other organisms 
LIMIT_JUMP_COVERAGE = 300
#these are the additional parameters to Celera Assembler; do not worry about performance, number or processors or batch sizes -- these are computed automatically. 
#CABOG ASSEMBLY ONLY: set cgwErrorRate=0.25 for bacteria and 0.1<=cgwErrorRate<=0.15 for other organisms.
CA_PARAMETERS =  cgwErrorRate=0.15
#CABOG ASSEMBLY ONLY: whether to attempt to close gaps in scaffolds with Illumina  or long read data
CLOSE_GAPS=1
#number of cpus to use, set this to the number of CPUs/threads per node you will be using
NUM_THREADS = 70
#this is mandatory jellyfish hash size -- a safe value is estimated_genome_size*20
JF_SIZE = 9000000000
#ILLUMINA ONLY. Set this to 1 to use SOAPdenovo contigging/scaffolding module.  
#Assembly will be worse but will run faster. Useful for very large (>=8Gbp) genomes from Illumina-only data
SOAP_ASSEMBLY=0
#If you are doing Hybrid Illumina paired end + Nanopore/PacBio assembly ONLY (no Illumina mate pairs or OTHER frg files).  
#Set this to 1 to use Flye assembler for final assembly of corrected mega-reads.  
#A lot faster than CABOG, AND QUALITY IS THE SAME OR BETTER.   
#DO NOT use if you have less than 20x coverage by long reads.
FLYE_ASSEMBLY=0
END

```

```bash
./assemble.sh 
[Thu 11 Nov 2021 09:21:59 PM PST] Processing pe library reads
[Thu 11 Nov 2021 09:21:59 PM PST] Processing sj library reads
[Thu 11 Nov 2021 09:21:59 PM PST] Average PE read length 123
[Thu 11 Nov 2021 09:22:01 PM PST] Using kmer size of 67 for the graph
[Thu 11 Nov 2021 09:22:01 PM PST] MIN_Q_CHAR: 33
WARNING: JF_SIZE set too low, increasing JF_SIZE to at least 12191908150, this automatic increase may be not enough!
[Thu 11 Nov 2021 09:22:01 PM PST] Estimated genome size: 756081379
[Thu 11 Nov 2021 09:22:01 PM PST] Computing super reads from PE 
flye 0
[Thu 11 Nov 2021 09:22:01 PM PST] Using CABOG from /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/bin/../CA8/Linux-amd64/bin
[Thu 11 Nov 2021 09:22:01 PM PST] Running mega-reads correction/assembly
[Thu 11 Nov 2021 09:22:01 PM PST] Using mer size 17 for mapping, B=15, d=0.02
[Thu 11 Nov 2021 09:22:01 PM PST] Estimated Genome Size 756081379
[Thu 11 Nov 2021 09:22:01 PM PST] Estimated Ploidy 2
[Thu 11 Nov 2021 09:22:01 PM PST] Using 70 threads
[Thu 11 Nov 2021 09:22:01 PM PST] Output prefix mr.67.17.15.0.02
[Thu 11 Nov 2021 09:22:01 PM PST] Pre-correcting long reads
[Thu 11 Nov 2021 10:09:42 PM PST] Pre-corrected reads are in longest_reads.25x.fa
[Thu 11 Nov 2021 10:12:07 PM PST] Computing mega-reads
[Thu 11 Nov 2021 10:12:07 PM PST] Running locally in 1 batch
[Fri 12 Nov 2021 10:26:15 AM PST] Refining alignments
[Fri 12 Nov 2021 11:17:49 AM PST] Computing allowed merges
[Fri 12 Nov 2021 11:21:47 AM PST] Joining
[Fri 12 Nov 2021 11:30:12 AM PST] Gap consensus
[Fri 12 Nov 2021 11:39:18 AM PST] Generating assembly input files
[Fri 12 Nov 2021 01:51:42 PM PST] Coverage threshold for splitting unitigs is 37 minimum ovl 63
[Fri 12 Nov 2021 01:51:42 PM PST] Running assembly
[Wed 17 Nov 2021 01:29:06 PM PST] Mega-reads initial assembly complete
[Wed 17 Nov 2021 01:29:06 PM PST] Closing gaps in scaffolds
[Wed 17 Nov 2021 01:34:34 PM PST] Aligning the reads to the contigs
[Wed 17 Nov 2021 01:39:22 PM PST] Filtering alignments
[Wed 17 Nov 2021 01:39:34 PM PST] Extracting reads for the patches
[Wed 17 Nov 2021 01:40:04 PM PST] Creating scaffold links
[Wed 17 Nov 2021 01:40:05 PM PST] Aligning the scaffolding sequences to the contigs
[Wed 17 Nov 2021 01:40:48 PM PST] Creating scaffold graph and building scaffolds
[Wed 17 Nov 2021 01:40:52 PM PST] Output scaffold sequences in genome.scf.joined.fa.scaffolds.fa
[Wed 17 Nov 2021 01:40:53 PM PST] Gap closing done
[Wed 17 Nov 2021 01:40:53 PM PST] Removing redundant scaffolds
[Wed 17 Nov 2021 02:37:10 PM PST] Assembly complete, primary scaffold sequences are in CA.mr.67.17.15.0.02/primary.genome.scf.fasta
[Wed 17 Nov 2021 02:37:10 PM PST] redundant or haplotype variant scaffold sequences are in CA.mr.67.17.15.0.02/alternative.genome.scf.fasta
[Wed 17 Nov 2021 02:37:10 PM PST] All done
[Wed 17 Nov 2021 02:37:10 PM PST] Final stats for CA.mr.67.17.15.0.02/primary.genome.scf.fasta
N50 811009
Sequence 632348718
Average 436404
E-size 1.20851e+06
Count 1449
```

# Summary Statistics 

## Masurca

```bash
stats.sh scaffolds.ref.fa 
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3136	0.1863	0.1865	0.3136	0.0002	0.0000	0.0000	0.3728	0.0308

Main genome scaffold total:         	7683
Main genome contig total:           	9989
Main genome scaffold sequence total:	958.223 MB
Main genome contig sequence total:  	957.992 MB  	0.024% gap
Main genome scaffold N/L50:         	483/466.004 KB
Main genome contig N/L50:           	599/349.301 KB
Main genome scaffold N/L90:         	2778/58.96 KB
Main genome contig N/L90:           	3831/42.594 KB
Max scaffold length:                	6.648 MB
Max contig length:                  	6.587 MB
Number of scaffolds > 50 KB:        	3065
% main genome in scaffolds > 50 KB: 	91.63%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	         7,683	         9,989	   958,223,014	   957,992,414	  99.98%
    500 	         7,683	         9,989	   958,223,014	   957,992,414	  99.98%
   1 KB 	         7,680	         9,984	   958,221,282	   957,990,882	  99.98%
 2.5 KB 	         7,344	         9,645	   957,623,103	   957,393,003	  99.98%
   5 KB 	         6,931	         9,225	   956,109,886	   955,880,486	  99.98%
  10 KB 	         6,111	         8,382	   949,880,495	   949,653,395	  99.98%
  25 KB 	         4,199	         6,379	   918,906,605	   918,688,605	  99.98%
  50 KB 	         3,065	         5,049	   878,030,062	   877,831,662	  99.98%
 100 KB 	         1,983	         3,609	   801,189,709	   801,027,109	  99.98%
 250 KB 	           945	         1,899	   635,729,820	   635,634,420	  99.99%
 500 KB 	           442	           964	   459,699,482	   459,647,282	  99.99%
   1 MB 	           145	           353	   254,446,844	   254,426,044	  99.99%
 2.5 MB 	            18	            50	    66,147,771	    66,144,571	 100.00%
   5 MB 	             3	             8	    17,017,011	    17,016,511	 100.00%
```

```bash
stats.sh primary.genome.scf.fasta 
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3139	0.1861	0.1862	0.3138	0.0002	0.0000	0.0000	0.3723	0.0355

Main genome scaffold total:         	1449
Main genome contig total:           	2535
Main genome scaffold sequence total:	632.349 MB
Main genome contig sequence total:  	632.240 MB  	0.017% gap
Main genome scaffold N/L50:         	214/811.009 KB
Main genome contig N/L50:           	257/647.248 KB
Main genome scaffold N/L90:         	737/272.952 KB
Main genome contig N/L90:           	978/169.229 KB
Max scaffold length:                	6.648 MB
Max contig length:                  	6.587 MB
Number of scaffolds > 50 KB:        	1092
% main genome in scaffolds > 50 KB: 	99.57%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	         1,449	         2,535	   632,348,718	   632,240,118	  99.98%
    100 	         1,449	         2,535	   632,348,718	   632,240,118	  99.98%
    250 	         1,360	         2,446	   632,331,983	   632,223,383	  99.98%
    500 	         1,322	         2,406	   632,319,759	   632,211,359	  99.98%
   1 KB 	         1,318	         2,400	   632,317,527	   632,209,327	  99.98%
 2.5 KB 	         1,213	         2,292	   632,144,004	   632,036,104	  99.98%
   5 KB 	         1,185	         2,262	   632,053,323	   631,945,623	  99.98%
  10 KB 	         1,173	         2,236	   631,958,137	   631,851,837	  99.98%
  25 KB 	         1,140	         2,163	   631,385,044	   631,282,744	  99.98%
  50 KB 	         1,092	         2,076	   629,641,715	   629,543,315	  99.98%
 100 KB 	         1,021	         1,970	   624,406,589	   624,311,689	  99.98%
 250 KB 	           787	         1,589	   582,268,195	   582,187,995	  99.99%
 500 KB 	           434	           951	   454,961,728	   454,910,028	  99.99%
   1 MB 	           145	           353	   254,446,844	   254,426,044	  99.99%
 2.5 MB 	            18	            50	    66,147,771	    66,144,571	 100.00%
   5 MB 	             3	             8	    17,017,011	    17,016,511	 100.00%
```


## Platanus-allee

```bash
stats.sh ajap_step10_consensus_consensusScaffold.fa 
A	C	G	T	N	IUPAC	Other	GC	GC_stdev
0.3125	0.1876	0.1877	0.3122	0.1304	0.0000	0.0000	0.3753	0.0718

Main genome scaffold total:         	335252
Main genome contig total:           	521912
Main genome scaffold sequence total:	1007.413 MB
Main genome contig sequence total:  	876.014 MB  	13.043% gap
Main genome scaffold N/L50:         	823/187.203 KB
Main genome contig N/L50:           	42739/5.177 KB
Main genome scaffold N/L90:         	50871/1.845 KB
Main genome contig N/L90:           	202792/825
Max scaffold length:                	4.162 MB
Max contig length:                  	143.859 KB
Number of scaffolds > 50 KB:        	1654
% main genome in scaffolds > 50 KB: 	57.98%


Minimum 	Number        	Number        	Total         	Total         	Scaffold
Scaffold	of            	of            	Scaffold      	Contig        	Contig  
Length  	Scaffolds     	Contigs       	Length        	Length        	Coverage
--------	--------------	--------------	--------------	--------------	--------
    All 	       335,252	       521,912	 1,007,412,658	   876,013,836	  86.96%
    100 	       335,252	       521,912	 1,007,412,658	   876,013,836	  86.96%
    250 	       143,319	       329,978	   978,162,844	   846,764,032	  86.57%
    500 	       112,269	       297,219	   966,875,323	   835,550,998	  86.42%
   1 KB 	        75,664	       253,918	   940,674,588	   809,552,051	  86.06%
 2.5 KB 	        40,173	       200,852	   883,700,280	   752,895,143	  85.20%
   5 KB 	        21,863	       165,549	   819,428,561	   689,210,290	  84.11%
  10 KB 	        10,911	       136,784	   743,213,957	   617,284,358	  83.06%
  25 KB 	         2,684	       102,463	   618,333,236	   512,013,942	  82.81%
  50 KB 	         1,654	        94,300	   584,064,857	   485,895,706	  83.19%
 100 KB 	         1,155	        86,989	   549,010,605	   458,203,116	  83.46%
 250 KB 	           678	        73,365	   471,932,927	   394,432,510	  83.58%
 500 KB 	           386	        56,855	   368,293,782	   308,210,955	  83.69%
   1 MB 	           127	        28,445	   185,984,088	   155,605,973	  83.67%
 2.5 MB 	             5	         2,418	    16,362,123	    13,709,936	  83.79%
```