---
title: "K-mer completeness and consensus quality assessment of genome assemblies"
toc: true
toc_sticky: true
layout: single
permalink: /merqury_assembly/

gallery:
  - url: /assets/images/busco/busco_analysis.png
    image_path: /assets/images/busco/busco_analysis.png
    title: "Comparison of buscos between genome assemblies"
    caption: 

---



```bash
$MERQURY/best_k.sh 1100000000 

# results
genome: 1100000000
tolerable collision rate: 0.001
19.9996

# iterate over the fastq files in that specific folder and create meryl databases
for file in /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/*.fastq
do
    # 1. Build meryl dbs
    meryl k=20 count output $file.meryl $file
done

# 2. Merge
meryl union-sum \
  output c_heheva.rawdata.meryl \
  /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/*.meryl

# checking genome assemblies

# Raven assembly
merqury.sh \
	/home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
	/home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/raven/raven_c_heheva.fasta \
	raven_c_heheva_merqury_analysis

# shasta assembly
merqury.sh \
	/home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
  /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta/Assembly.fasta \
	shasta_c_heheva_merqury_analysis

# Flye assembly
merqury.sh \
	/home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
  /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye/assembly.fasta \
	flye_c_heheva_merqury_analysis

# Published assembly
merqury.sh \
	/home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/c_heheva.rawdata.meryl \
  /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/15302229/Chiridota_heheva.fa \
	og_c_heheva_merqury_analysis

```
