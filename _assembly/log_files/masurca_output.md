---

title: "MaSuRCA Configuration File"
toc: true
toc_sticky: true
layout: single
permalink: /masurca_output/

---

See below for the command line output from running MaSuRCA

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