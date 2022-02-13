---
title: "Pangenomes"
type: posts
header:
  teaser: /assets/images/blog/2021-01-02/pangenome.png

---
### [The Need for a Human Pangenome Reference Sequence](https://www.annualreviews.org/doi/abs/10.1146/annurev-genom-120120-081921)

I was scrolling through articles in researcher-app on my phone the other day and an article title caught my eye: [The Need for a Human Pangenome Reference Sequence](https://www.annualreviews.org/doi/abs/10.1146/annurev-genom-120120-081921). Over the past five years, I have been occasionally check where things are at in pangenomics. It is obvious that the future of genomics and genetics will be pangenomics. As we acquire more genomes for individuals of one species, we will need a better way to compare them than using vcf files and genome alignments. However, there are barriers before this can be achieved.

The first is the cost and ease of sequencing has to come down even more. We are at a point now with sequencing costs that an average person in the US can afford to have their whole genome sequenced. One [website](https://sequencing.com) will do it for $400, although my customer experience with them was not great. As a master’s student, I sequenced a sea cucumber genome to a fairly high coverage of 120X for $1200. So getting a genome of any particular species is possible. But this is mostly short read sequencing using Illumina and a far cry from a pangenome. Additionally, a lot of really important information is lost when the genome is fragmented. One example is that of the Y chromosome that contains long palindromic sequences. Because they are palindromes, they have a unfortunate habit of folding back on themselves, aligning with other palindromes in the Y-chromosomes and recombining, or duplicating, or deleting themselves. Now these palindromes also contain functional genes between them. This can result in duplications or other modifications culminating in each male having a slightly different Y chromosome including different number of specific genes. Copy number variation can be lost during short read sequencing and assembling as the assembler algorithm is unable to tell that there are multiple copies of a gene adjacent to it. 

That's where long read sequencing comes in. There are a number of technologies available for long read sequencing. However, they are all too expensive to be viable for pangenomics. Currently in the research pipeline though, is something that is very promising: [solid state nanopore sequencing](https://pubmed.ncbi.nlm.nih.gov/31420594/). These promise to revolutionize genome sequencing if they every become a reality. They would be cheap, reliable, and most importantly, reusable. Meaning, you could go out into the woods, extract dna from a whole population of individuals, and sequence them on the spot without the need for expensive equipment or consumable reagents. When this becomes a reality, pangenomics will finally be a practical field of study. 

The second barrier to pangenomics is visualization. I am no expert in pangenomics, but I have yet to see a pangenome graph or figure that works. At the root of this problem is how to store and analyze pangenomic data. Bacterial genomes are small enough that brute force is fine. But thousands or millions of diploid (6 gigabases of data) human genomes is going to be a tough data beast to wrangle. This struggle is discussed in [The Need for a Human Pangenome Reference Sequence](https://www.annualreviews.org/doi/abs/10.1146/annurev-genom-120120-081921) but it is clear this hurdle is just now starting to be addressed fully. In another review published in 2020 called [Pangenome Graphs](https://www.annualreviews.org/doi/abs/10.1146/annurev-genom-120219-080406) they discuss various approaches to storing and analyzing pangenomic data, including visualization. But they are quick to note that "The visualization of pangenome graphs presents significant challenges that have proven to be difficult to resolve in a single system". 

Even though these hurdles are significant, I am excited to see what the next decade brings. The application of pangenomics is enormous and will change how we think about genomes, genetics, evolution, population structures etc. I am also cautiously hopeful it will allow for a better species concept and taxonomic classification. This would be a great boon to biology and systematics as genetics/genomics has upended many branches of the tree of life. In some cases rightfully so, in others maybe the lumpers and splitters are getting a little wild. Having a pangenome for a whole population would allow for diversity while also allowing taxonomists to accurately differentiate between populations that are different enough to be called two separate species. 

I also fully expect that pangenomes will change how we view genomic diversity. My hunch says we don't know the true extent of how different our genomes are from each other. Not necessarily in terms of gene content. But rather repetitive elements which directly impact regulation or copy number variations and inversions due to recombination events. Learning how diverse we are even in closely related people would be fascinating, insightful, and have serious implications for the practice of medicine. 

P.S. I think "pangenome" is a terrible word. Rather something like "species genome", "population genome", "type genome", or pretty much anything else that has more context in the name would be better. 

### References

[The Need for a Human Pangenome Reference Sequence](https://www.annualreviews.org/doi/abs/10.1146/annurev-genom-120120-081921)  
Karen H. Miga and Ting Wang  
Annual Review of Genomics and Human Genetics 2021 22:1, 81-102 

[Pangenome Graphs](https://www.annualreviews.org/doi/abs/10.1146/annurev-genom-120219-080406)  
Jordan M. Eizenga, Adam M. Novak, Jonas A. Sibbesen, Simon Heumos, Ali Ghaffaari, Glenn Hickey, Xian Chang, Josiah D. Seaman, Robin Rounthwaite, Jana Ebler, Mikko Rautiainen, Shilpa Garg, Benedict Paten, Tobias Marschall, Jouni Sirén, Erik Garrison  
Annual Review of Genomics and Human Genetics 2020 21:1, 139-162 

[solid state nanopore sequencing](https://pubmed.ncbi.nlm.nih.gov/31420594/)  
Goto Y, Akahori R, Yanagi I, Takeda KI.  
J Hum Genet. 2020 Jan;65(1):69-77. doi: 10.1038/s10038-019-0655-8. Epub 2019 Aug 16. PMID: 31420594.
