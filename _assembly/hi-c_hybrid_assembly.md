---

title: "Using Hi-C to scaffold long reads"
permalink: /hi-c/
layout: single
toc: true 
toc_sticky: true

sidebar:
  nav: sidebar-main
header:
  overlay_image: /assets/images/home/cuke1.jpg

excerpt: "The Short and Long"

---

currently thinking of using raven to assemble the long reads, do some polishing, and then scaffold using hi-c 

[qc3C: reference-free quality control for Hi-C sequencing data](https://www.biorxiv.org/content/10.1101/2021.02.24.432586v1.abstract)

```bash
#installing qc3C
conda create -y -n qc3c -c cerebis -c conda-forge -c bioconda qc3C
```


[Multifaceted Hi-C benchmarking: what makes a difference in chromosome-scale genome scaffolding?](https://academic.oup.com/gigascience/article/9/1/giz158/5695848?login=true)

[Integrating Hi-C links with assembly graphs for chromosome-scale assembly](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1007273)

[Extended haplotype-phasing of long-read de novo genome assemblies using Hi-C](https://www.nature.com/articles/s41467-020-20536-y)

[Technical considerations in Hi-C scaffolding and evaluation of chromosome-scale genome assemblies](https://www.authorea.com/doi/full/10.22541/au.162183528.83085003)

[Practical guide for obtaining and validating chromosome-scale genome assemblies with Hi-C scaffolding](https://europepmc.org/article/ppr/ppr260225)


[instaGRAAL: chromosome-level quality scaffolding of genomes using a proximity ligation-based scaffolder](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02041-z)

hmmm, salsa2, 3d-dna, and instagraal

what about haploid hi-c

[GraphUnzip: unzipping assembly graphs with long reads and Hi-C](https://www.biorxiv.org/content/10.1101/2021.01.29.428779v1.abstract)

[Extended haplotype-phasing of long-read de novo genome assemblies using Hi-C](https://www.nature.com/articles/s41467-020-20536-y)


[Chromosome Modeling on Downsampled Hi-C Maps Enhances the Compartmentalization Signal](https://pubs.acs.org/doi/abs/10.1021/acs.jpcb.1c04174)



https://open-michrom.readthedocs.io/en/latest/OpenMiChroM.html


a good hi-c tool list
https://www.sciencedirect.com/science/article/pii/S1672022918304339


[juicebox](https://github.com/aidenlab/Juicebox)

[gitbook juicebox tutorial](https://aidenlab.gitbook.io/juicebox/)
[github juicebox tutorial](https://github.com/aidenlab/Juicebox/wiki)

https://phasegenomics.github.io/2019/09/19/hic-alignment-and-qc.html

has some good info
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8120939/

"The main processing procedure of raw Hi-C data consists of four steps: alignment, filtering, binning and normalization [10], [11]. The first step is the alignment of the raw reads on a reference genome, and then proceed with the detection and filtering of valid interaction products, so that the analysis carries on the retained high-quality sequences [12]. In the third step, the whole genome is partitioned into small equal-sized regions (usually called bins), inside which the counting number of paired-end reads was reserved to evaluate the observed interactions between genomic loci, thereby transforming the dataset into a square symmetric matrix that also known as raw contact matrix. Specifically, each bin corresponds to each row or each column in the matrix, and the size of each bin measured by the unit kilobase (kb) is called the resolution of Hi-C contact matrix."