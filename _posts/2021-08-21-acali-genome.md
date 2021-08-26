---
title: "The genome of Apostichopus californicus"
type: posts
header:
  teaser: /assets/images/redundans_platanus-allee_step10_ajap-reference.fa.snail.png

---

I submitted the draft genome assembly of *Apostichopus californicus* to NCBI today. 

### Perspective
It feels as if I have completed my master's thesis. It might be a little premature as a rough draft of my thesis is currently being reviewed and I haven't published a paper on the genome. But uploading the genome to NCBI feels like I have contributed to science. To hearld this achievement, I want to share some things I've learned during this process. 

### First: Never trust eukaryote gene models in public databases unless they are from a model organism
Gene prediction is still very much a wild west. In model organisms, people have manually curated significant portions of the genome, and even then mistakes are still being found (references). Scientists who sequence non-model organisms don't have an army of people to manually go through potential genes and fix the gene models. So instead, they resort to using de-novo gene predictions tools that use a mix of ortholog databases and gene expression data to try and identify putative genes and generate gene models. These tools are incredible, but they come with a giant caveat - you can't trust their results without visualizing the gene model and checking it against expression data or ortholog alignment. 

Case in point: *A japonicus* Telomerase reverse transcriptase (aka TERT). This gene is labeled as telomerase on NCBI. I found it in *A californicus* (acali) and downloaded the purple sea urchin TERT and threw them into an alignment, it was wild. It was pretty clear the ajap TERT protein was truncated. So I jumped onto NCBI and sure enough, there was another gene immediately adjacent to the ajap TERT. When aligned, it matched the acali and conserved regions of the purple sea urchin TERT. I aligned a boat load of ajap rna-seq expression data to the ajap genome and loaded the genome and the alignment into Apollo to see what was going on. There appears to be dozens of assembly deletion errors in the genome which resulted in a shifted reading frame and inaccurate start/stop codons.

The fun part of this is the danger of inaccurate gene models. Because the ajap genome and gene models are currently the best for sea cucumbers and one of the best for echinoderms in general, these genes are included in the standard databases such as uniprot and refseq. These databases are then used for gene prediction in other species. Meaning this inaccurate gene model will potentially be propagated into other species resulting in a compounding mistakes. That being said, I haven't checked to see if the other echinoderm TERT gene models are incorrect. 

Knowing this about gene models has led me to question the validity of papers that claim to have analyzed large numbers of sequences and found such and such a result. Tools such as orthofinder are incredible, but in an age when most genomes are not well assembled and lack quality gene models, how much can you really trust the results? 

### Second: Everyone has an opinion on what the best tool or method is. No one really knows. 
I don't think this caveat is unique to bioinformatics. Science is rif with opinions and egos (mine included). It makes doing science a little more difficult when trying to tip toe around someones ego or maybe you don't and just burn the bridge down. Someone recently summarized it perfectly: The more time someone spends on their work and the more inconsequently the work is, the more..uh passiontate they will be about it. 
 
### Third: It's important to ask yourself what's good enough
As it became clear that I was not going to have enough long read data to get a high quality genome, it also became clear that there would be some limitations to how this genome can be used. 80% complete BUSCOs is not shabby for an assembly using short reads. This likely means the genome will probably be good for looking up gene sequences for use in cloning or PCR. The amount of fragmentation has me wondering how useful it will be for something like differential gene expression though. That being said, this is good enough. I achieved my personal goals of learning genome assembly and annotation and contributing to science. The genome is still useful and trying to further improve it without new sequence data would not be the best use of time and resources. Rather, I'll try to find further funding and in the meantime work on other sea cucumber related projects such as polishing the ajap genome and finding the sea cucumber telomerase RNA.

### Fourth: The most important skill a bioinformatician should have is knowing how to use duckduckgo
Bioinformatics is rapidly changing. New tools are published every day. What was standard practice a year ago won't be today. Professors and PIs will be largely oblivious to this as they don't have time to stay on top of how what the new best shinniest program is for assemblying genomes or even know that the field has changed. Case in point is genome sequencing. It was unimaginable even a few years ago for a genome sequencing project to be doable from a budget and resource standpoint. In another five years, it could very well be standard practice for undergraduates to sequence whole genomes for class projects. As a bioinformatician then, your best skill to stay on top of changes and find what is needed for a project is being able to go online and find the papers or github repository or stackexchange threads where people are actively discussing these things. In essence, learn how to teach yourself. 