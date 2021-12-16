---
title: "Flye Commandline Output"
permalink: /flye_output/
excerpt: ''
---

```bash
flye \
> --nano-raw /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
> --out-dir /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye \
> --threads 30
[2021-10-08 09:47:05] INFO: Starting Flye 2.9-b1768
[2021-10-08 09:47:05] INFO: >>>STAGE: configure
[2021-10-08 09:47:05] INFO: Configuring run
[2021-10-08 09:53:49] INFO: Total read length: 42430388059
[2021-10-08 09:53:49] INFO: Reads N50/N90: 22295 / 7832
[2021-10-08 09:53:49] INFO: Minimum overlap set to 8000
[2021-10-08 09:53:50] INFO: >>>STAGE: assembly
[2021-10-08 09:53:50] INFO: Assembling disjointigs
[2021-10-08 09:53:50] INFO: Reading sequences
[2021-10-08 09:57:51] INFO: Counting k-mers:
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-08 10:31:20] INFO: Filling index table (1/2)
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-08 11:04:40] INFO: Filling index table (2/2)
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-08 11:47:11] INFO: Extending reads
[2021-10-08 12:23:56] INFO: Overlap-based coverage: 18
[2021-10-08 12:23:56] INFO: Median overlap divergence: 0.143962
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-09 08:44:31] INFO: Assembled 11024 disjointigs
[2021-10-09 08:44:56] INFO: Generating sequence
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-09 08:56:36] INFO: Filtering contained disjointigs
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-09 09:10:44] INFO: Contained seqs: 1210
[2021-10-09 09:12:43] INFO: >>>STAGE: consensus
[2021-10-09 09:12:43] INFO: Running Minimap2
[2021-10-09 13:52:59] INFO: Computing consensus
[2021-10-09 15:39:46] INFO: Alignment error rate: 0.171719
[2021-10-09 15:40:26] INFO: >>>STAGE: repeat
[2021-10-09 15:40:26] INFO: Building and resolving repeat graph
[2021-10-09 15:40:27] INFO: Parsing disjointigs
[2021-10-09 15:40:50] INFO: Building repeat graph
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-09 18:16:44] INFO: Median overlap divergence: 0.0934004
[2021-10-09 18:19:47] INFO: Parsing reads
[2021-10-09 18:24:47] INFO: Aligning reads to the graph
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-09 23:41:49] INFO: Aligned read sequence: 35139080130 / 37968005402 (0.925492)
[2021-10-09 23:41:49] INFO: Median overlap divergence: 0.0628257
[2021-10-09 23:42:22] INFO: Mean edge coverage: 29
[2021-10-09 23:42:24] INFO: Simplifying the graph
[2021-10-09 23:54:25] INFO: >>>STAGE: contigger
[2021-10-09 23:54:25] INFO: Generating contigs
[2021-10-09 23:54:25] INFO: Reading sequences
[2021-10-10 00:02:11] INFO: Generated 28210 contigs
[2021-10-10 00:02:33] INFO: Added 161 scaffold connections
[2021-10-10 00:03:59] INFO: >>>STAGE: polishing
[2021-10-10 00:03:59] INFO: Polishing genome (1/1)
[2021-10-10 00:03:59] INFO: Running minimap2
[2021-10-10 03:58:09] INFO: Separating alignment into bubbles
[2021-10-10 05:10:21] INFO: Alignment error rate: 0.134724
[2021-10-10 05:10:21] INFO: Correcting bubbles
0% 10% 20% 30% 40% 50% 60% 70% 80% 90% 100% 
[2021-10-11 01:51:23] INFO: >>>STAGE: finalize
[2021-10-11 01:51:56] INFO: Assembly statistics:

	Total length:	1540823993
	Fragments:	16728
	Fragments N50:	307072
	Largest frg:	3605186
	Scaffolds:	0
	Mean coverage:	23

[2021-10-11 01:51:56] INFO: Final assembly: /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye/assembly.fasta

```