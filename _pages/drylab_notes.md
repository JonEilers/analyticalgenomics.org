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
