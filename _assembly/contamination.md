---
title: "Genome Assembly Contamination"
toc: true
toc_sticky: true
layout: single
permalink: /contamination/

excerpt: "We believe your genome might be infected"

gallery:
  - url: /assets/images/assembly/summary_statistics/ajap.snail.png
    image_path: /assets/images/assembly/summary_statistics/ajap.snail.png
    title: The Published Apostichopus californicus Genome Assembly 
  - url: /assets/images/assembly/summary_statistics/ajap-masurca.snail.png
    image_path: /assets/images/assembly/summary_statistics/ajap-masurca.snail.png
    title: Apostichopus japonicus MaSURCA assembly using publicly available data
  - url: /assets/images/assembly/summary_statistics/ajap-masurca-scaffolds.ref.fa.snail.png
    image_path: /assets/images/assembly/summary_statistics/ajap-masurca-scaffolds.ref.fa.snail.png
    title: Apostichopus japonicus MaSURCA assembly containing lower confidence contigs

---

# Introduction

[Blobtoolkit](https://blobtoolkit.genomehubs.org/) is an amazing suite of tools for analyzing and producing publication grade figures of genome assemblies. There [publication](https://academic.oup.com/g3journal/article/10/4/1361/6026202) contains examples and explanations of the various figures and analysis' that can be performed. This includes snail plots for visulizing genome summary statistics, blobplots of contamination and taxonomy hits, and cumulative assembly span plots of the multiple genome assemblies. 

# Installing

There are a number of options for how to install blobtoolkit. There is a [docker image](https://blobtoolkit.genomehubs.org/install/use-docker/) available. Additionally, you can install it directly by following these [instruction](https://blobtoolkit.genomehubs.org/install/), but as they note you can make your life easier by installing most of the dependencies using conda. 

I have used both the docker image and installing directly. Both are a bit of a pain to do. Thankfully, they introduced an alternative recently - using [pip](https://pypi.org/project/pip/). pip is a package installer for python. It is akin to using conda to install stuff. At the time of writing this I suggest you use pip. 

## Installing blobtoolkit

Installing using pip
```bash
pip install blobtoolkit
```

That was easy and painless. Love it. 

## Downloading and setting up databases

To fully utilize blobtoolkit you will need to download a few databases. They go over how to do this in detail on their [website](https://blobtoolkit.genomehubs.org/install/). But in case you don't feel like clicking on the above link I have also copied and pasted their code below. 

downloading the taxonomy database
```bash
mkdir -p taxdump;
cd taxdump;
curl -L ftp://ftp.ncbi.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz | tar xzf -;
cd ..;
```

The NCBI [taxonomy database](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=Root) is just that. It contains a list of organism names, unique ID numbers, and how they are all related to the best of our knowledge. This will be used in blobtoolkit when looking for contamination in the genome assembly. 

downloading the ncbi nucleotide database
```bash
# NCBI nucleotide database
mkdir -p nt
wget "ftp://ftp.ncbi.nlm.nih.gov/blast/db/nt.??.tar.gz" -P nt/ && \
        for file in nt/*.tar.gz; \
            do tar xf $file -C nt && rm $file; \
        done
```

The NCBI [nucleotide databse](https://www.ncbi.nlm.nih.gov/nuccore) aka ncbi nt database contains "entries from all traditional divisions of GenBank, EMBL, and DDBJ; excluding bulk divisions (gss, sts, pat, est, htg) and wgs entries. Partially non-redundant" - per the [readme](https://ftp.ncbi.nlm.nih.gov/blast/db/README) file on the ftp webpage. This will be used to search the genome assembly for sequences that might not be sea cuke sequences. 

Downloading the uniprot protein database
```bash

# before you download and format the uniprot database you will need to
# install the sequence aligner called diamond.
conda create -n uniprot
conda activate uniprot
conda install -c bioconda diamond 


# Uniprot database
mkdir -p uniprot
wget -q -O uniprot/reference_proteomes.tar.gz \
 ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/$(curl \
     -vs ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/ 2>&1 | \
     awk '/tar.gz/ {print $9}')
cd uniprot
tar xf reference_proteomes.tar.gz

touch reference_proteomes.fasta.gz
find . -mindepth 2 | grep "fasta.gz" | grep -v 'DNA' | grep -v 'additional' | xargs cat >> reference_proteomes.fasta.gz

echo -e "accession\taccession.version\ttaxid\tgi" > reference_proteomes.taxid_map
zcat */*/*.idmapping.gz | grep "NCBI_TaxID" | awk '{print $1 "\t" $1 "\t" $3 "\t" 0}' >> reference_proteomes.taxid_map

diamond makedb -p 16 --in reference_proteomes.fasta.gz --taxonmap reference_proteomes.taxid_map --taxonnodes ../taxdump/nodes.dmp -d reference_proteomes.dmnd
cd -
```
"The Universal Protein Resource (UniProt), a collaboration between the European
Bioinformatics Institute (EBI), the SIB Swiss Institute of Bioinformatics, and
the Protein Information Resource (PIR), is comprised of three databases, each
optimized for different uses. The UniProt Knowledgebase (UniProtKB) is the
central access point for extensively curated protein information, including
function, classification and cross-references. The UniProt Reference Clusters
(UniRef) combine closely related sequences into a single record to speed up
sequence similarity searches. The UniProt Archive (UniParc) is a comprehensive
repository of all protein sequences, consisting only of unique identifiers and
sequences.
"

The above is an excerpt taken from the [readme](https://ftp.ebi.ac.uk/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/) file on their ftp server. In this case we are using the reference proteomes which includes Archaea, Bacteria, Eukaryota and Viruses

# Running

running the docker image on the apostichopus japonicus genome assembly in order to create the appropriate assembly files for running other blobtools later.
```bash
blobtools create \
  --fasta /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
  --taxid 307972 \
  --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
  /home/jon/Working_Files/blobtoolkit/ajap

blobtools create \
  --fasta /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
  --taxid 307972 \
  --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
  /home/jon/Working_Files/blobtoolkit/ajap-masurca

blobtools create \
  --fasta /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
  --taxid 307972 \
  --taxdump /home/jon/Working_Files/blobtoolkit/taxdump \
  /home/jon/Working_Files/blobtoolkit/ajap-masurca-scaffolds.ref.fa

```

running minimap for generating coverage data. There is no raw read data for the original assembly, so this will only be for the two masurca assemblies. Am also only using the Pacbio data as there is far too much short read illumina data.

this will require minimap2 and samtools. I suggest you make a conda environment and install both using conda. Also note - I am using the commands from the blobtools website. 
```bash
conda create -n blobtools
conda activate blobtools
conda install -c bioconda minimap2 
conda install -c bioconda samtools

minimap2 -ax map-pb \
         -t 20 \
         /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
         /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fasta \
| samtools sort -@20 -O BAM -o ajap-masurca -

minimap2 -ax map-pb \
         -t 20 \
         /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
         /home/jon/Working_Files/sea_cuke_species_data/apostichopus_japonicus/apostichopus_japonicus_raw_genome_seq_data/SRR6282347.fasta \
| samtools sort -@20 -O BAM -o Aajap-masurca-scaffolds.ref.bam -
```

blasting NCBI nucleotide database against the genomes
```bash
# install the blast package to your blobtools conda environment
conda install -c blast

# run blastn on the three assemblies
blastn \
  -db nt \
  -query /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/primary.genome.scf.fasta \
  -outfmt "6 qseqid staxids bitscore std" \
  -max_target_seqs 10 \
  -max_hsps 1 \
  -evalue 1e-25 \
  -num_threads 16 \
  -out ajap-masurca.ncbi.blastn.out

blastn -db nt \
       -query /home/jon/Working_Files/japonicus_genome_project/MaSuRCA-4.0.5/masurca_results/scaffolds.ref.fa \
       -outfmt "6 qseqid staxids bitscore std" \
       -max_target_seqs 10 \
       -max_hsps 1 \
       -evalue 1e-25 \
       -num_threads 16 \
       -out ajap-masurca.scaffolds.ref.ncbi.blastn.out

blastn -db nt \
       -query /home/jon/Working_Files/genome_assemblies/japonicus/Ajap_genome.fasta \
       -outfmt "6 qseqid staxids bitscore std" \
       -max_target_seqs 10 \
       -max_hsps 1 \
       -evalue 1e-25 \
       -num_threads 16 \
       -out ajap.ncbi.blastn.out
```

viewing the results locally
```bash
blobtools view --local /home/jon/Working_Files/blobtoolkit/
```

# Results