---
title: "Escaping Evolution"
layout: posts
header:
  teaser: /assets/images/blog/2021-10-28/escape.jpg
toc: false
excerpt: "I'm Free!"
---

# Gene overlap protects against mutation

Genetic engineering might be the only way to save life on our planet as we know it. Between climate change and introduction of invasive species, native species are going to have a difficult time adapting and competing. Additionally, being able to rapidly modify crops to better suite a changing environment will be a powerful tool. 

History tells us that nothing can escape evolution. Given enough time, mutations will arise and genes will become mutated, give birth, and die. This is a problem if you are introducing genes or modifying genes in an organism and you want it to keep that gene intact. 

A [paper](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1009475) titled "Engineering gene overlaps to sustain genetic constructs in vivo" seems to have found one way of accomplishing this by overlapping the reading frames of two genes. The first gene is an essential gene and the second is the genetic modification. A "costly" mutation in the first gene would not only disable it, but also the second gene. If the first gene is something that is critical to the survival or function of the organism then any mutation would be selected against and by association with that gene, the second gene would be preserved. Or at least part of it, specifically the translation initiation motif. They also found via simulation that larger overlaps should confer greater protection. Optimizing parameters could result in up to 90% protection in the overlap region. 

While looking for genes which would allow for overlap in E. coli, they found only 20% of the genes would accommodate it. However, changing one amino acid in a protein resulted in upwards of 80% of genes be available for overlap. They then checked this in COGs and found a similar pattern. They also checked in the [International genetically Engineered Machine](https://igem.org/Registry) registry and found similar results. 

However, the Wikipedia article on [gene overlap](https://en.wikipedia.org/wiki/Overlapping_gene) paints a more nuanced picture in which gene overlap is not completely protective against mutation. One paper that was cited on Wikipedia is titled [Birth and death of gene overlaps in vertebrates](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2151771/) in which they look at gene overlaps in several model organisms to see how well conserved they are. Their conclusion is that they are not. Although it appears they were looking at all types of gene overlap and not just cases in which the translation initiation site overlapped with essential genes.

The authors introduced a gene overlap into E. coli and tested their predictions and found them to be fairly accurate. The authors also note that this construct/method does not confer 100% protection against mutation and that exposure to non-lab conditions might select for polar mutations which are less protected against in this approach. Still bloody incredible. 

See below for a figure outlining their approach.

{% include figure image_path="/assets/images/blog/2021-10-28/overlapping_genes.png" %}

