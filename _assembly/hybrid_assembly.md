---

title: "Hybrid assembly using Platanus-allee and MaSuRCA"
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

*Apostichopus japonicus*, also called the Japanese sea cucumber is found in the eastern coast of russia, china, korea, and around japan. It is an important fishery and model organism for studying regeneration. The official NCBI reference genome was published in [2017](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.2003790) with another lower quality assembly published a year before this.  

While I was investigating the structure of telomerase in *Apostichopus californicus* I found that the telomerase in *Apostichopus japonicus* was fragmented due to numerous assembly errors. There were enough errors that it was not going to be an easy process to manually correct them. So I decided it would make for a good hybrid genome assembly example. 

I looked for the raw data that the authors should have made publicly available and found that they uploaded the rna-seq expression data but none of the data for the genome assembly. I thought this was rather odd and emailed the authors about it, but never received a reply. Thankfully, another study, [Sea cucumber genome provides insights into saponin biosynthesis and aestivation regulation](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6018497/) published shortly after this one also included a large amount of raw sequence data that they used for genome assembly. Amusingly, they never published their genome assembly. 

Hybrid genome assembly projects use multiple data types in the assembly process. This can range from several different lengths of illumina short read data to oxford nanopore or pacbio long read data, 10x-chromium, optical mapping, hi-c etc. Essentially, if you are using more than one data type you will be doing hybrid assembly. For the last ten or so years, this has been the standard for achieving the best possible genome assembly. 

# The Data

See below for data links and data type. 

180bp paired end - [SRR6251237](https://www.ncbi.nlm.nih.gov/sra/SRR6251237)  
350bp paired end - [SRR6255867](https://www.ncbi.nlm.nih.gov/sra/SRR6255867)  
450bp paired end - [SRR6255870](https://www.ncbi.nlm.nih.gov/sra/SRR6255870) 
500bp paired end - [SRR6255868](https://www.ncbi.nlm.nih.gov/sra/SRR6255868) 
5kb long insert mate-pair - [SRR6257769](https://www.ncbi.nlm.nih.gov/sra/SRR6257769)  
10kb long insert mate-pair - [SRR6257771](https://www.ncbi.nlm.nih.gov/sra/SRR6257771)  
15kb long insert mate-pair - [SRR6257773](https://www.ncbi.nlm.nih.gov/sra/SRR6257773)  
20kb long-insert mate-pair - [SRR6257777](https://www.ncbi.nlm.nih.gov/sra/SRR6257777)  
pacbio clr long read - [SRR6282347](https://www.ncbi.nlm.nih.gov/sra/SRR6282347)  

# Hybrid Genome Assemblers

There a number of genome assembly tools that accept more than one data type. Two of my favorite are [platanus-allee](https://www.nature.com/articles/s41467-019-09575-2) and [MaSuRCA](https://academic.oup.com/bioinformatics/article/29/21/2669/195975?login=true). Both MaSuRCA and Platanus-allee require at least short read data but also accept long read data, and in the case of platanus-allee, 10x-chromium data. One significant difference between the two assembler is regarding genome phasing. A phased genome is when the assembler is able to distinguish the haplotypes of each chromosome pair. In the past, assemblers were designed to ignore ploidy levels as the sequence data was inadequate for telling chromosomes apart. This is not the case anymore. With long read sequencing and hi-c it is now possible to completely disintangle homologous chromsomes, although this is just beginning to become standard practice. 

Platanus-allee was designed specifically to be able to "phase" a genome assembly and then, if you wanted, collapse the phased assembly into a haploid assembly. To the best of my knowledge MaSuRCA does not have this ability yet. However, MaSuRCA is significantly faster and at least in this project, produced a more contigious assembly. Both tools are designed to be used with heterozygous genomes, which is important as the *Apostichopus japonicus* has a heterzygousity of 1.59%, meaning that both sets of chromosomes are different enough to confuse a genome assembler. This is important to know as genome assemblers will sometimes duplicate heterozygous genes in the haploid genome assembly because it can't tell the genes are variants. 

## Platanus-allee assembler

[Platanus-allee](http://platanus.bio.titech.ac.jp/platanus2) has three stages. The first stage is diploid contig assembly. The second stage is where the assembly is collapsed into a haploid assembly. The final stage is scaffolding where linkage information is used to stitch pieces together and then gap filled.  

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

Run Time: almost three weeks
```bash
Started platanus-allee assemble: 14 Oct 2021
Started platanus-allee phase: 19 Oct 2021 
Started platanus-allee consensus: 02 Nov 2021
```

Memory usage was surprisingly not bad compared to when I have run it with just short read data. It peaked at 317 gigabytes of ram used. When running with just illumina data is maxed out my server ram (~400gb) and crashed. The author stated they thought it was probably a memory leak. Never figured it out and had to switch to using an older version of platanus-allee. 

## MaSuRCA assembler

MaSuRCA has a different requirement for running it. It requires a configuration bash script. The authors provide a template with some instructions and the user edits the configuration template to suite their needs. See below for the one I tweaked and used.

[MaSuRCA Configuration Script](/MaSuRCA_config/)   

After modifying the config file, you then execute it using bash ```./bin/MaSuRCA MaSuRCA_config_japonicus.txt```. This will output another shell script file called "assemble.sh". You'll do the same to this one: ```./assemble.sh```. And viola - sit back and wait. 

Quick notes: 
- I recently reinstalled ubuntu on my server. So some stuff was missing when I tried to run MaSuRCA including numactl, boost, and bzip libraries. 
- I also didn't keep track of memory usage, but it roughly matched what the authors say will be required. 
- Additionally, MaSuRCA will use a lot of hard drive space. For this run it was a little over a terabyte. 

[MaSuRCA command line output](/MaSuRCA_output/)

Final stats 
```bash
N50 811009
Sequence 632348718
Average 436404
E-size 1.20851e+06
Count 1449
```

Run time: Started the assembly on Nov 11 and finished on Nov 17 so about 7 days. 

# Summary Statistics 

## MaSuRCA

MaSuRCA outputs a few different scaffolds. The final results are in primary.genome.scf.fasta. Redundant or haplotype variant scaffold sequences are in alternative.genome.scf.fasta. Then there is scaffolds.ref.fa, which I am not entirely sure what it is, but I included it here as I was curious if it was just the two previously mentioned scaffolds combined. 

```bash
conda create -n bbmap
conda activate bbmap
conda install -c bioconda bbmap 

# running it on a single genome would use stats.sh
stats.sh genome_of_interest

# but I have three genome assemblies, so
statswrapper.sh \
	in=/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta,\
	/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/scaffolds.ref.fa,\
	/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/alternative.genome.scf.fasta \
	format=6

#n_scaffolds	n_contigs	scaf_bp	contig_bp	gap_pct	scaf_N50	scaf_L50	ctg_N50	ctg_L50	scaf_N90	scaf_L90	ctg_N90	ctg_L90	scaf_max	ctg_max	scaf_n_gt50K	scaf_pct_gt50K	gc_avg	gc_std	filename
1449	2535	632348718	632240118	0.017	214	811009	257	647248	737	272952	978	169229	6648447	6586636	1092	99.572	0.37229	0.03548	/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta
7683	9989	958223014	957992414	0.024	483	466004	599	349301	2778	58960	3831	42594	6648447	6586636	3065	91.631	0.37278	0.03083	/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/scaffolds.ref.fa
6363	7585	325904099	325781899	0.037	831	111108	1032	86740	3307	21871	4140	18511	692521	692521	1973	76.215	0.37373	0.03116	/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/alternative.genome.scf.fasta
```

but the columns are off so let's make it pretty. I pasted the results into a spreadsheet and then copied/pasted into this websites spreadsheet to markdown [converting tool](https://tabletomarkdown.com/convert-spreadsheet-to-markdown/):  

| #n\_scaffolds | n\_contigs | scaf\_bp  | contig\_bp | gap\_pct | scaf\_N50 | scaf\_L50 | ctg\_N50 | ctg\_L50 | scaf\_N90 | scaf\_L90 | ctg\_N90 | ctg\_L90 | scaf\_max | ctg\_max | scaf\_n\_gt50K | scaf\_pct\_gt50K | gc\_avg | gc\_std | filename                                                                                                           |
| ------------- | ---------- | --------- | ---------- | -------- | --------- | --------- | -------- | -------- | --------- | --------- | -------- | -------- | --------- | -------- | -------------- | ---------------- | ------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| 1449          | 2535       | 632348718 | 632240118  | 0.017    | 214       | 811009    | 257      | 647248   | 737       | 272952    | 978      | 169229   | 6648447   | 6586636  | 1092           | 99.572           | 0.37229 | 0.03548 | /home/jon/Working\_Files/japonicus\_genome\_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta     |
| 7683          | 9989       | 958223014 | 957992414  | 0.024    | 483       | 466004    | 599      | 349301   | 2778      | 58960     | 3831     | 42594    | 6648447   | 6586636  | 3065           | 91.631           | 0.37278 | 0.03083 | /home/jon/Working\_Files/japonicus\_genome\_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/scaffolds.ref.fa             |
| 6363          | 7585       | 325904099 | 325781899  | 0.037    | 831       | 111108    | 1032     | 86740    | 3307      | 21871     | 4140     | 18511    | 692521    | 692521   | 1973           | 76.215           | 0.37373 | 0.03116 | /home/jon/Working\_Files/japonicus\_genome\_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/alternative.genome.scf.fasta |

Note: as previously noted elsewhere, the N50 and L50 are swapped in this output. I really wish the authors of BBtools would fix this.

So the scaffold count for the first and second entries add up close to the 2nd entry but it's not a perfect match so I am hestitant to say the scaffold.ref.fa assembly is actually just the primary and alternative genomes merged. 

## Platanus-allee

At the end of the consensus step Platanus-allee outputs a scaffold assembly. See below for how that turned out. 

Quickly comparing the MaSuRCA and Platanus-allee assemlbies.
```bash
statswrapper.sh \
	in=/home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta,\
	/home/jon/Working_Files/japonicus_genome_project/platanus-allee/Platanus_allee_v2.2.2_Linux_x86_64/ajap_step10_consensus_consensusScaffold.fa,\
	format=6
```

| n\_scaffolds | n\_contigs | scaf\_bp   | contig\_bp | gap\_pct | scaf\_N50 | scaf\_L50 | ctg\_N50 | ctg\_L50 | scaf\_N90 | scaf\_L90 | ctg\_N90 | ctg\_L90 | scaf\_max | ctg\_max | scaf\_n\_gt50K | scaf\_pct\_gt50K | gc\_avg | gc\_std | filename                                                                                                                                                 |
| ------------ | ---------- | ---------- | ---------- | -------- | --------- | --------- | -------- | -------- | --------- | --------- | -------- | -------- | --------- | -------- | -------------- | ---------------- | ------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1449         | 2535       | 632348718  | 632240118  | 0.017    | 214       | 811009    | 257      | 647248   | 737       | 272952    | 978      | 169229   | 6648447   | 6586636  | 1092           | 99.572           | 0.37229 | 0.03548 | /home/jon/Working\_Files/japonicus\_genome\_project/MaSuRCA-4.0.5/CA.mr.67.17.15.0.02/primary.genome.scf.fasta                                           |
| 335252       | 521912     | 1007412658 | 876013836  | 13.043   | 823       | 187203    | 42739    | 5177     | 50871     | 1845      | 202792   | 825      | 4161996   | 143859   | 1654           | 57.977           | 0.37529 | 0.07183 | /home/jon/Working\_Files/japonicus\_genome\_project/platanus-allee/Platanus\_allee\_v2.2.2\_Linux\_x86\_64/ajap\_step10\_consensus\_consensusScaffold.fa |

As I've noted previously when using this tool, the L50 and N50 are switched. So the platanus-allee assembly has over 300k scaffolds, a scaffold n50 of 187k bp but a contig n50 of ~5k. That's not great, especially compared to MaSuRCA assembly. The low n50 might be due to a few hundred megabases of the assembly being reads shorter than 1kb as platanus-allee seems to try to keep enough reads so the assembly represents the whole genome and not just parts that it could assembly. But that's just a guess. I really won't be able to tell until I view these assemblies using a snailplot. 

# Summary

So the platanus-allee assembly has a lot more scaffolds/contigs, aka fragmented, but also contains more data in its assembly. I think this is because it tries to capture the whole genome in the assembly regardless of if it is contigious. MaSuRCA removes stuff shorter than a few hundred base pair or at least doesn't include very many short reads. Some might say that mean it loses information. While this is true, that extra information isn't very useful (I think?) and will greatly slow down steps later during genome masking, gene prediction, and annotation. MaSuRCA was also significantly faster than Platanus-allee. 

Even though it seems fairly obvious which assembly is better, a "best assembly" can't be declared without further evaluation of more assembly characteristics. This includes using tools such as Inspector or Mercury to look at assembly kmer properties, Blobtoolkit for generating graphs to look at genome contiguity and contamination, and Busco to check for gene fragmentation. 