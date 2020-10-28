---
title: "Dry Lab Thoughts and Notes"
permalink: /drylab_notes/
toc: true
toc_label: "Dry Lab Notes"

---


# Mon 19 Oct 2020 01:16:47 AM PDT

Note sure how to structure this yet. So for now I will use dates. 

## Current status
This is where I am at right now: 
- raw sequence data has been cleaned, both long read and short read.
- All the assemblers I have tried using require at least several fold coverage of long read to be useful. There is a tool available for manual curration of genome assembly contigs/scaffolds that I may look at eventually.
- I have a number of assemblies using Masurca and platanus-allee.
- I have tried haslr and ragout and they did not produce very complete assemblies.
- I have a previous redundans run using platanus-allee contigs from a kmer=20 run, I might try it with contigs from a kmer=10 run.
- Ragtag has some interesting tools for potentially improving the assemblies
- A paper on genome synteny noted that using different species as reference assemblies can impact assembly arrangements. but things like genes are probably fine. But I should take a look at that.

## future plans
Some thoughts on what I will try doing next:
- I should run redundans with the k=10 platanus-allee contigs. 
- I should try to find the masurca contig files too and do a run or two using redundans
- Look into how useful [ragtag](https://github.com/malonge/RagTag) will be for improving assemblies. Specifically the misassembly and scaffolding tools. Details in the github wiki.


# Tue 27 Oct 2020 11:33:30 PM PDT

So redundans finished. It took five days yeesh.... It's the end of october and I feel like I need to move on to the next step or I will be stuck working on assemblies forever. So I think I will just move on. So here is the current assembly list

## Raw Data
- short read illumina about 60 to 100 X coverage
- nanopore long read .05 x coverage sadly

## Assemblies
- Masurca
  - illumina without nanopore or reference genome
  - illumina and used a reference genome
  - illumina and nanopore with reference
  - illumina and nanopore without reference
- Platanus-allee version 2.0.2 
  - illumina and nanopore with k=20 
  - illumina only k=20
  - illumina only k=10
- Platanus
  - illumina only
- Redundans using Platanus-allee
  - platanus-allee k=20, illumina only diploid contigs, no nanopore, with raw illumina reads
  - platanus-allee k=10, illumina only diploid contigs, no nanopore, with raw illumina reads
  - tried with nanopore, and it said no alignments were found for those reads

other tools I have tried without satisfactory results
- ragout
- ragtag
- haslr
- Abyss

## What's next? 
Time to take a look at the genome assembly quality and pick one I guess.

tools I intend to use for this
- blobtoolkit (probably docker version if I can get it to work)
- the scripts used in this paper for checking sequence expansion/collapse: https://doi.org/10.1371/journal.pcbi.1008104 
- quast-lg (maybe, possibly include genemark?) 
- busco analysis
- kat
- mummer4
- nucdiff (if I can get it to work with mummer4 output)
- multiqc
- genomeQC 
- reapr
- squat 
- Referee

I am not sure which one I will start with. Probably blobtoolkit as it requires a lot of other useful preliminary steps such as mapping raw data back to the genomes, busco analysis, blasting the big databases, calculating assembly metrics, etc. After that I might take a wack at the sequence expansion/collapse tools