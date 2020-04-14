---
title: "Blobtools: Platanus-allee Assembly Analysis"
toc: true
toc_sticky: true
layout: single

gallery:
  - url: /assets/images/blobtools/blobtools_db_pcali_platanus.blobDB.json.bestsum.phylum.p7.span.100.covplot.cov0.png
    image_path: /assets/images/blobtools/blobtools_db_pcali_platanus.blobDB.json.bestsum.phylum.p7.span.100.covplot.cov0.png
    title: 
  - url: /assets/images/blobtools/pcali_cov_1k.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p15.span.1000.covplot.cov0.png
    image_path: /assets/images/blobtools/pcali_cov_1k.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p15.span.1000.covplot.cov0.png
    title: 

gallery2:
  - url: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.read_cov.cov0.png
    image_path: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.read_cov.cov0.png
    title: 
  - url: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.phylum.p7.span.100.blobplot.read_cov.cov0.png
    image_path: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.phylum.p7.span.100.blobplot.read_cov.cov0.png
    title: 

gallery3:
  - url: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.cumulative.cov0.1.Apostichopus_japonicus.png
    image_path: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.cumulative.cov0.1.Apostichopus_japonicus.png
    title: 
  - url: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.cumulative.cov0.5.Strongylocentrotus_purpuratus.png
    image_path: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.cumulative.cov0.5.Strongylocentrotus_purpuratus.png
    title: 
  - url: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.cumulative.cov0.12.Oryzias_melastigma.png
    image_path: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.1000.blobplot.cumulative.cov0.12.Oryzias_melastigma.png

gallery4:
  - url: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.3000.blobplot.cumulative.read_cov.cov0.png
    image_path: /assets/images/blobtools/pcali_plat.blobtools_db_pcali_platanus.blobDB.json.bestsum.species.p20.span.3000.blobplot.cumulative.read_cov.cov0.png
    title:             
---

## Blobtools
A modular command-line solution for visualisation, quality control and taxonomic partitioning of genome datasets

### Background
Excerpt from the blobtools [website](https://blobtools.readme.io/docs)

Reconstruction of target genomes from sequence data produced by instruments that are agnostic as to the species-of-origin may be confounded by contaminant DNA. Whether introduced during sample processing or through co-extraction alongside the target DNA, if insufficient care is taken during the assembly process, the final assembled genome may be a mixture of data from several species. Such assemblies can confound sequence-based biological inference and, when deposited in public databases, may be included in downstream analyses by users unaware of underlying problems.

We present BlobToolKit, a software suite to aid researchers in identifying and isolating non-target data in draft and publicly available genome assemblies. BlobToolKit can be used to process assembly, read and analysis files for fully reproducible interactive exploration in the browser-based Viewer. BlobToolKit can be used during assembly to filter non-target DNA, helping researchers produce assemblies with high biological credibility.

We have been running an automated BlobToolKit pipeline on eukaryotic assemblies publicly available in the International Nucleotide Sequence Data Collaboration and are making the results available through a public instance of the Viewer at https://blobtoolkit.genomehubs.org/view. We aim to complete analysis of all publicly available genomes and then maintain currency with the flow of new genomes. We have worked to embed these views into the presentation of genome assemblies at the European Nucleotide Archive, providing an indication of assembly quality alongside the public record with links out to allow full exploration in the Viewer.

### Installation
So I didn't realize this when I initially installed this, but apparently I used the first version of blobtools. The second and improved version of blobtools can be [here](https://blobtoolkit.genomehubs.org). At some point I should go back and use blobtools2. 

```
conda install -c bioconda blobtools 
```

conda package information
```
blobtools                 1.0.1                    py27_2    bioconda
```

### Commands


Download database
```
wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Reference_Proteomes_2017_07.tar.gz
```


Concatenate all protein sequences into uniprot_ref_proteomes.fasta
```
cat */*.fasta > uniprot_ref_proteomes.fasta
```


Simplify sequence IDs
```
cat uniprot_ref_proteomes.fasta | sed -r 's/(^>sp\|)|(^>tr\|)/>/g' | cut -f1 -d"|" > temp; mv temp uniprot_ref_proteomes.fasta
```


Unpack protein FASTAs for each kingdom
```
ls | grep "fasta" | grep -v 'DNA' | grep -v 'additional' | xargs gzip -d
ls | grep "idmapping" | xargs gzip -d
```


Subset mapping file to only contain NCBI TaxID entries
```
cat */*.idmapping | grep "NCBI_TaxID" > uniprot_ref_proteomes.taxids
```


Make Diamond database and blast against it
```
diamond makedb --taxonmap uniprot_ref_proteomes.taxids --in uniprot_ref_proteomes.fasta -d uniprot_ref_proteomes.diamond
diamond makedb -p 35 --in uniprot_ref_proteomes.fasta -d uniprot_ref_proteomes.diamond 
diamond blastx \
 --query out_consensusScaffold.fa \
 --db /home/jon/Working_Files/uniprot/Reference_Proteomes_2019_11/uniprot_ref_proteomes.diamond.dmnd \
 --outfmt 6 \
 --sensitive \
 --max-target-seqs 1 \
 --evalue 1e-25 \
 --threads 35
```


#### Building SNAP index and aligning sequence data to assembly

```
./snap-aligner index out_consensusScaffold.fa index/ -t35
./snap-aligner paired index ../P_cali_g_1.fq ../P_cali_g_2.fq -t 35  -o -bam output.bam
```

results
```
results:
Loading index from directory... 7s.  1181184926 bases, seed size 20
Aligning.
Total Reads    Aligned, MAPQ >= 10    Aligned, MAPQ < 10     Unaligned              Too Short/Too Many Ns  %Pairs Reads/s   Time in Aligner (s)
617,348,676    537,397,943 (87.05%)   29,184,958 (4.73%)     50,765,705 (8.22%)     70 (0.00%)             81.02%%  170,892   3,612
```

#### Blobtools Taxify

create a taxified diamond output file
```
blobtools taxify -f diamond.out -m /home/jon/Working_Files/uniprot/Reference_Proteomes_2019_11/uniprot_ref_proteomes.taxids -s 0 -t 2
```

#### blobtools map2cov

use blobtools map2cov to convert bam to cov file
```
blobtools map2cov -i out_consensusScaffold.fa -b output.bam -o output
```

#### blobtools create 

making the blobtools database thing
```
blobtools create -i out_consensusScaffold.fa -t diamond.taxified.out -t blast.out -x bestsumorder -c output.output.bam.cov -o blobtools_db_pcali_platanus
```

#### Generating blobtools plots

```
blobtools plot -i blobtools_db_pcali_platanus.blobDB.json -o pcali_plat -p 20 -l 1000 -r species 
```

#### creating blobtools view file

```
blobtools view -i blobtools_db_pcali_platanus.blobDB.json -o blobtools

grep '^##' blobtools.blobtools_db_pcali_platanus.blobDB.bestsum.table.txt ; grep -v '^##' 

blobtools.blobtools_db_pcali_platanus.blobDB.bestsum.table.txt | column -t -s $'\t' > pcali_blobtools.txt

blobtools view -r species -i blobtools_db_pcali_platanus.blobDB.json -o pcali_blob_species

grep '^##' blobtools.blobtools_db_pcali_platanus.blobDB.bestsum.table.txt ; grep -v '^##' 

blobtools.blobtools_db_pcali_platanus.blobDB.bestsum.table.txt | column -t -s $'\t' > pcali_species_blobtools.txt

blobtools plot -i blobtools_db_pcali_platanus.blobDB.json -o pcali_plat -p 20 -l 3000 -r species --cumulative
```

it does a bit better when fewer reads are filtered out
```
blobtools plot -i blobtools_db_pcali_platanus.blobDB.json -o pcali_plat -p 20 -l 1000 -r species --cumulative
```

#### blobtools covplot

```
blobtools covplot -i blobtools_db_pcali_platanus.blobDB.json -c output.output.bam.cov 

blobtools covplot -i blobtools_db_pcali_platanus.blobDB.json -c output.output.bam.cov -o pcali_cov_1k -l 1000 -r species  -p 15
```

results
```
[+] Reading BlobDB blobtools_db_pcali_platanus.blobDB.json
[+]   Loading BlobDB into memory ...
[+]   Deserialising BlobDB (using 'ujson' module) (this may take a while) ...
[+]   Finished in 218.156861067s
[+] Parsing cov_y_axis - output.output.bam.cov
[%]   100%
[+] Extracting data for plots ...
[I] Echinodermata : sequences = 69,408, span = 490.54 MB, N50 = 15,622 nt
[I] no-hit : sequences = 608,585, span = 326.63 MB, N50 = 2,248 nt
[I] Mollusca : sequences = 4,446, span = 9.5 MB, N50 = 2,907 nt
[I] Chordata : sequences = 1,216, span = 8.3 MB, N50 = 14,135 nt
[I] Cnidaria : sequences = 318, span = 1.33 MB, N50 = 8,887 nt
[I] Arthropoda : sequences = 465, span = 1.18 MB, N50 = 13,385 nt
[I] Brachiopoda : sequences = 46, span = 0.34 MB, N50 = 12,613 nt
[I] other : sequences = 269, span = 0.99 MB, N50 = 10,077 nt
[+] Plotting blobtools_db_pcali_platanus.blobDB.json.bestsum.phylum.p7.span.100.covplot.cov0.png
[+] Writing blobtools_db_pcali_platanus.blobDB.json.bestsum.phylum.p7.span.100.covplot.stats.txt
```

### Results

{%include gallery%}

{% include gallery id="gallery2" %}

{% include gallery id="gallery3" %}

{% include gallery id="gallery4" %}

