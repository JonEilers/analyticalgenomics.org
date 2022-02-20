---

title: "Genome Assembly Quality Assessment using Inspector"
permalink: /genome_quality/
layout: single
toc: true 
toc_sticky: true

excerpt: "Good day Sir, I am here to inspect your assembly."


gallery_merqury:
  - url: assets/images/assembly/assembly_evaluation/c_heheva_merqury_analysis.raven_c_heheva.spectra-cn.fl.png
    image_path: assets/images/assembly/assembly_evaluation/c_heheva_merqury_analysis.raven_c_heheva.spectra-cn.fl.png
    title: "Raven Assembly K-mer Spectra"
  - url: assets/images/assembly/assembly_evaluation/shasta_c_heheva_merqury_analysis.Assembly.spectra-cn.fl.png
    image_path: assets/images/assembly/assembly_evaluation/shasta_c_heheva_merqury_analysis.Assembly.spectra-cn.fl.png
    title: "Shasta Assembly K-mer Spectra"

---

# Introduction

There is a bad habit in the genome assembly world of not checking the quality of your assembly before publishing it. Part of that is due to a lack of tools. [Inspector](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02527-4) and [Merqury](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9) are two great tools to address this. Inspector uses long reads aligned to the genome to check quality. Merqury uses highly accurate short reads to analyze k-mer distribution in the assembly and raw data. 

# The Assemblies

Three *Chiridota heheva* assemblies previously created using the assemblers Shasta, Flye, and Raven will be used for comparison. Note that these assemblies used long read nanopore data that was not error corrected. Additionally, the assemblies have not been polished. For reference I have also included the published version of the genome [downloaded from NCBI](https://www.ncbi.nlm.nih.gov/datasets/genomes/?taxon=2743191&utm_source=gquery&utm_medium=referral). 


# Inspector

## Installation

Installation is fairly straight forward. Further instructions can be found at the Inspector [github](https://github.com/ChongLab/Inspector)

```bash
conda create -n inspector
conda activate inspector
conda install -c bioconda inspector
```

## Running Inspector

Running inspector requires a fasta/q file of long reads and a genome assembly to align the reads too. Additionally you can set an output directory and even add a reference genome to compare to. 

```bash

# Raven assembly
inspector.py \
    -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/raven/raven_c_heheva.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
    -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/raven \
    --datatype nanopore \
    -t 40 

# Shasta assembly
inspector.py \
    -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta/Assembly.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
    -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/shasta \
    --datatype nanopore \
    -t 40 

# Flye assembly
inspector.py \
    -c /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/flye/assembly.fasta \
    -r /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
    -o /home/jon/Working_Files/Chiridota_heheva_genome_project/inspector/flye \
    --datatype nanopore \
    -t 40 

# Published assembly

```

Note: Minimap uses a lot of power and will max out the UPS at 70 cpus (because m UPS doesn't provide as much power as the server can use. Price of being cheap.). This might be a python specific use of minimap. I do need to get a larger ups at some point

## Results

Inspector outputs a text document containing a summary of the results. Copying and pasting into excel and then into [excel to markdown table converter website](https://tableconvert.com/excel-to-markdown) generates the below table. 

| Statics of contigs:                | Raven Assembly | Shasta Assembly | Flye Assembly |
|------------------------------------|----------------|-----------------|---------------|
| Number of contigs                  | 3227           | 15199           | 16728         |
| Number of contigs > 10000 bp       | 3223           | 11941           | 13396         |
| Number of contigs >1000000 bp      | 207            | 28              | 179           |
| Total length                       | 1221957363     | 1209883028      | 1540823993    |
| Total length of contigs > 10000 bp | 1221922362     | 1196342865      | 1521394030    |
| Total length of contigs >1000000bp | 290685476      | 36833854        | 267358448     |
| Longest contig                     | 2901347        | 1748484         | 3605186       |
| Second longest contig length       | 2793378        | 1705511         | 3518942       |
| N50                                | 606319         | 201222          | 307072        |
| N50 of contigs >1Mbp               | 606319         | 201222          | 307072        |

The raven assembly is clearly the winner with regards to assembly contiguity and N50. The shasta and raven assemblies more or less agree on the size of the genome with Flye having a significantly larger assembly, which is suspicious. However, Flye also has the first and second longest contig. With just this information, Raven seems like it is probably the better assembly and the Flye assembly is suspect. But let's take a look at some more information output by Inspector.


| Read to Contig alignment:           | Raven Assembly | Shasta Assembly | Flye Assembly |
|-------------------------------------|----------------|-----------------|---------------|
| Mapping rate /%                     | 98.43          | 98.38           | 98.75         |
| Split-read rate /%                  | 27.4           | 30.49           | 21            |
| Depth                               | 34.5491        | 35.2633         | 27.7466       |
| Mapping rate in large contigs /%    | 21.93          | 2.89            | 18.42         |
| Split-read rate in large contigs /% | 23.79          | 20.73           | 16.53         |
| Depth in large contigs               | 32.8825        | 34.2379         | 29.7275       |

Long read mapping rate was close to the same for all three. Split-read rate, which I am interpreting as long read sequences that had to be split in order to map successfully is honestly kinda high for all three. Flye has the lowest, but 21% seems like a lot of the reads are having to be split in order to map successfully and they probably shouldn't need to be at all. For high quality genomes I would expect that number to be low. This indicates to me two things: First that splitting might be due to genome fragmentation, and second that there are a significant number of errors in the assembly that are causing the reads to not map well, resulting in needing to split the reads. Split read rate in large contigs is lower than for the whole assembly and likely has a similar explanation. 

30X coverage is generally considered the minimum for assembling a long read genome succesfully. All three assemblies are pretty close to this, although Flye is a bit under probably due to a larger assembly size. Coverage is higher in large contigs but also similar to overall assembly coverage. I would view this as a good thing. If coverage was noticably different in the large contigs relative to the rest of the assembly, this might suggest higher assembly errors, greater fragmentation in the rest of the genome, or the genome contains a lot of repetitive elements that are interferring with assembly. 

Mapping rate in large contigs I am assuming means percentage of total reads that mapped to large contigs. This metric is likely constrained by how many "large contigs" are in the assembly. So Shasta having fewer might be a result of not having very many "large contigs". It could also be a consequence of there being a lot of assembly errors in the the shasta assembly. 


| Error type       | Raven Assembly | Shasta Assembly | Flye Assembly |
|------------------|----------------|-----------------|---------------|
| Structural error | 4587           | 347             | 729           |
| Expansion        | 379            | 220             | 38            |
| Collapse         | 288            | 48              | 4             |
| Haplotype switch | 3920           | 79              | 686           |
| Inversion        | 0              | 0               | 1             |


Now for the juicy part. The raven assembly, by a large margin, has the most large scale errors in the assembly. However, the majority of these errors are "haplotype switchs". Once those are removed from the total, it appears that the raven assembly is not that much worse than the others, but still worse. These errors can also be easily corrected using polishing tools such as Hapo-g or the one built into Inspector. 

| Indels                              | Raven Assembly   | Shasta Assembly  | Flye Assembly    |
|-------------------------------------|------------------|------------------|------------------|
| Small-scale assembly error /per Mbp | 2161.13566796317 | 6993.43411054656 | 188.174131326123 |
| Total small-scale assembly error    | 2640740          | 8366545          | 286287           |
| Base substitution                   | 1240424          | 2714250          | 134786           |
| Small-scale expansion               | 818823           | 4165783          | 103520           |
| Small-scale collapse                | 581493           | 1486512          | 47981            |
|                                     |                  |                  |                  |
| QV                                  | 25.2858950332793 | 21.3835831443883 | 34.6460926406124 |


This is probably the most important section regarding genome quality. In my experience, small scale errors such as insertions, deletions, and base substitutions significantly impact gene prediction. These small errors can change the reading frames of the gene which results in gene truncations or wrong gene models. Keeping that in mind, Flye is the clear winner with regards to genome quality. This is reflected in the much higher QV score than the other two. The QV score is a [PHRED score](https://en.wikipedia.org/wiki/Phred_quality_score) assigned to the genome which tells you how frequently errors occur in the genome. In this case a phred score of 20 corresponds to roughly 1 error every 100 bases and a phred score of 30 suggests an error every 1000 bases. For context, the best human genome has a phred score somewhere around 60 to 70. 

To summarize the results: While the raven assembly is longer and more contigious, it contains a large number of errors whereas the Flye assembly is more fragmented, suspciously longer, but contains fewer errors. Before declaring a winning genome assembly though we need to do more analysis. Next up is Merqury. 

Note: need to run this on the original genome too. 

# Merqury 

[Merqury](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9) compares assembly k-mers to k-mers found in the short-read data that was used in creating the assembly. It is able to estimate assembly accuracy and completeness in addition to phase block continuity and haplotype switch errors. It also provides a phred score (QV) that is similar to the one produced by Inspector.

## Installation

Installation is fairly straight forward using conda. Additionally, if you want to install directly, more instructions can be found at their [github repository](https://github.com/marbl/merqury)

```bash
conda create -n merqury
conda activate merqury
conda create -n merqury -c conda-forge -c bioconda merqury openjdk=11
```

## Running Merqury
```bash
# this is a simple script to calculate what the ideal k-mer size is based on the estimated genome size
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

# 2. Merge the paired-end fastq meryl databases
meryl union-sum \
  output c_heheva.rawdata.meryl \
  /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466782/*.meryl

# Running Merqury on the genome assemblies

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

## Results


| Genome Assembly | Assembly only kmers | Shared Kmers | QV      | Error Rate |
|-----------------|---------------------|--------------|---------|------------|
| Raven           | 187445697           | 1221896050   | 20.8134 | 0.00829211 |
| Shasta          | 287105937           | 1209594249   | 18.7105 | 0.0134569  |

Shared kmers are those kmers that are found in both the read dataset and the genome assembly. Consensus quality (QV) is the Mercury Phred quality score. The error rate is calculated using presense/absence of k-mers in the assembly vs raw data. See the Merqury vs Inspector QV scores section for more info on this is calculated. It looks like shasta has a lower QV and higher error rate than the Raven assembly. This agrees with the Inspector results. Although they are a little off from each other. 

| Genome Assembly | Assembly K-mers | Read K-mers | Completeness Percent |
|-----------------|-----------------|-------------|----------------------|
| raven_c_heheva  | 597225562       | 904653194   | 66.0171              |
| Shasta          | 545857355       | 904653194   | 60.3389              |

Per the [paper](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9#Sec10), completeness is defined as the "the fraction of reliable k-mers in the read set that are also found in the assembly. For repetitive genomes, erroneous read set k-mers can sometimes appear above this threshold due to recurring errors in high-copy repeat families, but this is rare."

{% include gallery id="gallery_merqury" caption="Merqury Spectra graphs of the Raven and Shasta assemblies"  %}

Per the [paper](), "A typical k-mer spectrum for a heterozygous diploid genome consists of two primary peaks, one representing k-mers that are 1-copy in the diploid genome (heterozygous, on a single haplotype) and one representing those that are 2-copy in the diploid genome (homozygous, on both haplotypes or two copies on one haplotype). The 2-copy k-mers appear with a frequency approximately equal to the average depth of sequencing coverage, where the 1-copy k-mers appear with frequency approximately equal to half the sequencing coverage. **If a genome is entirely homozygous, only the 2-copy peak may appear, and if the genome is extremely heterozygous, only the 1-copy peak may appear**. With sufficient sequencing coverage (to separate the peaks along the axis), and a proper choice of k, both peaks are visible for most genomes."

So it appears, not suprisingly, that this sea cucumber genome is highly hetrozygous. When comparing the graphs, note that the axis scales are different. I need to see if this can be fixed so the scales are the same, otherwise it is hard to determine how different the kmer distribution is between assemblies. 

# Merqury vs Inspector QV scores

QV scores are [Phred quality scores](https://en.wikipedia.org/wiki/Phred_quality_score) assigned to a genome assembly and are traditionally calculated using

$$\mathrm{QV}=-10{\log}_{10}\ E$$

Where E is the error rate per a base in the genome assembly or data being sequenced. Both Inspector and Merqury have different ways of calculating the error rate in an assembly. Merqury compares the presence of k-mers in the assembly to those in the raw data. Inspector aligns long reads to the assembly, identifies and sums the assembly errors which is then used to create a probability of error per a base. See below for a more detailed look at the math behind these calculations. 

## Inspector QV

Inspector calculates the Phred score differently than Merqury. Although they state is correlates well with Merqury QV results. 

$${N}_{Err}={N}_{Exp}+{N}_{Col}+{N}_{Her}+{N}_{Small}+{n}_{Inv}$$

$${N}_{Err}$$ is the sum total of errors (number of bases) inspector found. 

$$E=\frac{N_{\mathrm{Err}}}{N_{\mathrm{asm}}}=\frac{N_{\mathrm{Exp}}+{N}_{\mathrm{Col}}+{N}_{\mathrm{Her}}+{N}_{\mathrm{Small}}+{n}_{\mathrm{Inv}}}{N_{\mathrm{asm}}}$$

$$E$$ or error rate is the number of incorrect bases over the total number bases in the assembly.

$$\mathrm{QV}=-10{\log}_{10}\ E$$

Phred scores (QV) are the log of the error rate as shown above. For more detailed information, check out the methods section of the inspector [paper](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02527-4#Sec12)

## Merqury QV

$$\mathrm{QV}=-10{\log}_{10}\ E$$

As stated earlier, a phred score is the -10log of the error rate as shown above. The probability that a base is correct is calculated as shown below where E is

$$E=1-P=1-{\left(1-\frac{K_{\mathrm{asm}}}{K_{\mathrm{total}}}\right)}^{\frac{1}{\mathrm{k}}}$$

which is 1 minus the probability that a base is correct. The probability that is a base is correct is calculated as 

$$P={\left(\frac{K_{\mathrm{shared}}}{K_{\mathrm{total}}}\right)}^{\frac{1}{\mathrm{k}}}$$

As noted in the [paper](), "the read set is assumed to completely cover the genome, any k-mer found only in the assembly $${(K_{asm} = K_{total} − K_{shared})}$$ likely reflects a base error in the assembly consensus." Some basic math is done to rearrange the equation with $${(K_{asm} = K_{total} − K_{shared})}$$ plugged in and viola

$$P={\left(1-\frac{K_{\mathrm{asm}}}{K_{\mathrm{total}}}\right)}^{\frac{1}{\mathrm{k}}}$$

While Inspector ane Merqury have very different methods of calculating error rates, the Inspector paper notes they correlate well. Although in this example, the QV for the assemblies is lower than that calculated using Inspector, but fairly close. For more information on Merqury methods see their [paper](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02134-9#Sec10)