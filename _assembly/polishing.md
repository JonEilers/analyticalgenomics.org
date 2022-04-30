---
title: "Genome Polishing"
permalink: /genome_polishing/
excerpt: "Arts and sciences are not cast in a mould, but are formed and perfected by degrees, by often handling and polishing, as bears leisurely lick their cubs into form. - Michel De Montaigne"

---

Genome assembly algorithms are not perfect. Especially long read genome assemblies that have noisy data. Insertions and deletions (aka indels) and adapter contamination can sneak their way into the genome assembly. This can be problem as it can mess up de novo gene prediction resulting in truncated genes or shifted reading frames. If the naive molecular geneticist then downloads these genes and tries to use them for wet lab research they'll find themselves very frustrated and confused. 

But there is an easy way to deal with this problem. It's called genome assembly polishing. 


Polishing: polish the new ajap assembly, new stichopus assembly? 
tools: inspector, hapo-g, nextpolish, ntEdit


inspector
```bash
conda create -n inspector
conda activate inspector
conda install -c bioconda inspector

```


```bash
# high quality masurca assembly
inspector.py \
    -c /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
    -t 40 \
    --datatype clr 

# correcting the assembly
inspector-correct.py \
    -i /home/jon/Working_Files/japonicus_genome_project/inspector/adenovo_evaluation-out/ \
    --datatype pacbio-raw \
    -t 40 \
    -o ajap_masurca.inspector_corrected/ 

# checking the quality of the corrected assembly
inspector.py \
    -c /home/jon/Working_Files/japonicus_genome_project/inspector/ajap_masurca.inspector_corrected/contig_corrected.fa \
    -r /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
    -t 40 \
    --datatype clr 
```




Note: the hapo-g [paper](https://academic.oup.com/nargab/article/3/2/lqab034/6262629?login=false) recommends using nextpolish for the first round and then doing 6 more rounds of hapo-g. I did not do this. 

installing and running hapo-g
```bash
conda create -n hapog
conda activate hapog
conda install -c lbgb_cea hapog

# round 1 long reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \   
  --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
  -o round_1_long \              
  -t 36 \                     
  -u                         

# round 2 long reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_1_long/hapog_results/hapog.fasta \ 
  --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \ 
  -o round_2_long \ 
  -t 36 \ 
  -u   

# round 3 long reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_2_long/hapog_results/hapog.fasta \ 
  --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \ 
  -o round_3_long \ 
  -t 36 \ 
  -u 

# round 4 long reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_3_long/hapog_results/hapog.fasta \
  --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
  -o round_4_long \
  -t 36 \
  -u 

# round 5 long reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_4_long/hapog_results/hapog.fasta \
  --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
  -o round_5_long \
  -t 36 \
  -u 

# round 6 long reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_5_long/hapog_results/hapog.fasta \
  --single /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
  -o round_6_long \
  -t 36 \
  -u 

```

short read 
```bash

# round 1 short reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
  --pe1 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
  --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
  -o round_1_short \
  -t 36 \
  -u  

# round 2 short reads 
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_1_short/hapog_results/hapog.fasta \
  --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
  --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
  -o round_2_short \
  -t 36 \
  -u 

# round 3 short reads 
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_2_short/hapog_results/hapog.fasta \
  --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
  --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
  -o round_3_short \
  -t 36 \
  -u

# round 4 short reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_3_short/hapog_results/hapog.fasta \
  --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
  --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
  -o round_4_short \
  -t 36 \
  -u

# round 5 short reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_4_short/hapog_results/hapog.fasta \
  --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
  --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
  -o round_5_short \
  -t 36 \
  -u  

# round 6 short reads
hapog.py \
  --genome /home/jon/Working_Files/japonicus_genome_project/hapog/round_5_short/hapog_results/hapog.fasta \
  --pe1  /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_1.fastq \
  --pe2 /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6255867_2.fastq \
  -o round_6_short \
  -t 36 \
  -u

```

hapo-g doesn't provide any result summaries. However, we can run inspector or mercury on the final results to get a QV. hapo-g also outputs a summary tsv file which can be dumped into excel or pandas to generate some graphs of what those changes look like. 

For the sake of not dumping too much none-relavent commands here, I won't include the inspector/merqury commands. 

just use excel for making some graphs?

take a peak at the bam files using jbrowse2

run busco on the final two? 


running inspector and merqury on the final two assemblies
```bash

conda activate inspector


inspector.py \
    -c /home/jon/Working_Files/japonicus_genome_project/hapog/long_read/round_6_long/hapog_results/hapog.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fastq \
    -t 40 \
    --datatype clr 
```