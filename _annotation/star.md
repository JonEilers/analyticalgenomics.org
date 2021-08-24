---
title: "STAR: RNA-seq Mapping"
toc: true
toc_sticky: true
layout: single

---

## STAR

Mapping rna-seq to a genome assembly

### Background

Excerpt from the bbtools [website](https://jgi.doe.gov/data-and-tools/bbtools/)

BBTools is a suite of fast, multithreaded bioinformatics tools designed for analysis of DNA and RNA sequence data. BBTools can handle common sequencing file formats such as fastq, fasta, sam, scarf, fasta+qual, compressed or raw, with autodetection of quality encoding and interleaving. It is written in Java and works on any platform supporting Java, including Linux, MacOS, and Microsoft Windows and Linux; there are no dependencies other than Java (version 7 or higher). Program descriptions and options are shown when running the shell scripts with no parameters.

Excerpt from the STAR [publication](https://www.ncbi.nlm.nih.gov/pubmed/23104886)

To align our large (>80 billon reads) ENCODE Transcriptome RNA-seq dataset, we developed the Spliced Transcripts Alignment to a Reference (STAR) software based on a previously undescribed RNA-seq alignment algorithm that uses sequential maximum mappable seed search in uncompressed suffix arrays followed by seed clustering and stitching procedure. STAR outperforms other aligners by a factor of >50 in mapping speed, aligning to the human genome 550 million 2 × 76 bp paired-end reads per hour on a modest 12-core server, while at the same time improving alignment sensitivity and precision. In addition to unbiased de novo detection of canonical junctions, STAR can discover non-canonical splices and chimeric (fusion) transcripts, and is also capable of mapping full-length RNA sequences. Using Roche 454 sequencing of reverse transcription polymerase chain reaction amplicons, we experimentally validated 1960 novel intergenic splice junctions with an 80-90% success rate, corroborating the high precision of the STAR mapping strategy.

### Installation

STAR and bbmap conda version
```bash
star                      2.7.3a                        0    bioconda
bbmap                     38.73                h516909a_0    bioconda
```

Installing
```bash
conda install -c bioconda star 
conda install -c bioconda bbmap 
```

### Commands

filtered reads out of the genome that were shorter than 1kbp
```bash
bbduk.sh in=/home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked out=pcali_ajap.redundans.platanus.masked.filter-1k.fa minlen=1000
```

Indexing
```bash
STAR --runThreadN 75 \
--runMode genomeGenerate \
--genomeDir /home/jon/Working_Files/star \
--genomeFastaFiles /home/jon/Working_Files/pcali_ajap.redundans.platanus.masked.filter-1k.fa \
--limitGenomeGenerateRAM 223523654698
```

Runing star with index
```bash
STAR --runThreadN 75 \
--genomeDir /home/jon/Working_Files/star/ \
--readFilesIn /home/jon/Working_Files/Apostichopus_californicus_rna-seq/SRR1139198_1.trim.fq,/home/jon/Working_Files/Apostichopus_californicus_rna-seq/SRR1695477_1.trim.maq15.fq /home/jon/Working_Files/Apostichopus_californicus_rna-seq/SRR1139198_2.trim.fq,/home/jon/Working_Files/Apostichopus_californicus_rna-seq/SRR1695477_2.trim.maq15.fq \
--outSAMtype BAM SortedByCoordinate 
```


### Results

results for filtering out reads
```bash
0.057 seconds.
Initial:
Memory: max=78953m, total=78953m, free=78869m, used=84m

Warning: Did not find expected fasta file extension for filename /home/jon/Working_Files/repeats/repeatmasking_output/pcali_ajap.redundans.platanus.fa.masked
Input is being processed as unpaired
Started output streams:	0.036 seconds.
Processing time:   		12.599 seconds.

Input:                  	304189 reads 		878550047 bases.
Total Removed:          	226785 reads (74.55%) 	88909575 bases (10.12%)
Result:                 	77404 reads (25.45%) 	789640472 bases (89.88%)

Time:                         	12.637 seconds.
Reads Processed:        304k 	24.07k reads/sec
Bases Processed:        878m 	69.52m bases/sec
```

results from mapping rna-seq to the filtered redundans genome assembly
```bash
                                 Started job on |	Feb 07 20:25:36
                             Started mapping on |	Feb 07 20:26:02
                                    Finished on |	Feb 07 20:33:22
       Mapping speed, Million of reads per hour |	348.11

                          Number of input reads |	42546497
                      Average input read length |	199
                                    UNIQUE READS:
                   Uniquely mapped reads number |	35299295
                        Uniquely mapped reads % |	82.97%
                          Average mapped length |	196.00
                       Number of splices: Total |	7974712
            Number of splices: Annotated (sjdb) |	0
                       Number of splices: GT/AG |	7837703
                       Number of splices: GC/AG |	23861
                       Number of splices: AT/AC |	4074
               Number of splices: Non-canonical |	109074
                      Mismatch rate per base, % |	1.05%
                         Deletion rate per base |	0.07%
                        Deletion average length |	2.37
                        Insertion rate per base |	0.07%
                       Insertion average length |	1.48
                             MULTI-MAPPING READS:
        Number of reads mapped to multiple loci |	3612519
             % of reads mapped to multiple loci |	8.49%
        Number of reads mapped to too many loci |	31594
             % of reads mapped to too many loci |	0.07%
                                  UNMAPPED READS:
  Number of reads unmapped: too many mismatches |	0
       % of reads unmapped: too many mismatches |	0.00%
            Number of reads unmapped: too short |	3485525
                 % of reads unmapped: too short |	8.19%
                Number of reads unmapped: other |	117564
                     % of reads unmapped: other |	0.28%
                                  CHIMERIC READS:
                       Number of chimeric reads |	0
                            % of chimeric reads |	0.00%
```