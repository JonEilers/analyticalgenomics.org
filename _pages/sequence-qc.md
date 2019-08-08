---
title: "Raw Sequence QC"
permalink: /sequenceQC/

feature_row:
  - image_path: /assets/images/extraction.jpg
    title: "DNA Extraction"
    excerpt: "Links to information on DNA Extraction Protocols, techniques, etc"
    url: "/dnaextraction/"
  - image_path: /assets/images/sequencing.jpg
    title: "DNA Sequencing"
    excerpt: "Links to information on DNA Sequencing, techniques, etc"
    url: "/dnasequencing/"
  - image_path: /assets/images/assembly.jpg
    title: "Genome Assembly"
    excerpt: "Links to information on Genome Assembly"
    url: "/genomeassembly/"

---


Below are a list of tools for performing quality control of sequence data. All of these tools can be downloaded using [conda](https://docs.conda.io/en/latest/). 

* [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) is a tool for assessing raw sequence quality.
* [Kat](https://www.earlham.ac.uk/kat-tools) is a suite of tools for generating, analyzing, and comparing k-mer spectra produced from sequence data. Spectra includes determing sequence completeness, sequence bias, identifying contaminants, validating assemblies. 
* [Jellyfish](https://www.cbcb.umd.edu/software/jellyfish/) is a tool for counting k-mers in sequence data
* [GenomeScope](http://qb.cshl.edu/genomescope/) is both a R package and website which estimates genome heterozygosity, repeat content, and size using kmer based statistical approaches
* [kmergenie](http://kmergenie.bx.psu.edu/) is a tool for estimating the best k-mer length for de novo genome assembly



{% include feature_row %}
