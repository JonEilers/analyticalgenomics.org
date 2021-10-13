---
title: GWAS and Yamanaka factors
permalink: /genereg/
excerpt: "The butterfly effect in gene regulation"
type: posts
header:
  teaser: /assets/images/2021-10-11/chromatin.png
toc: false

---

### Reconciling the disparate results of GWAS and induced pluripotency

The other day I was scrolling through articles on [researcher-app](https://www.researcher-app.com/) when I stumbled across this beauty of a paper:[complex small world regulatory networks emerge from the 3d organisation of the human](https://www.nature.com/articles/s41467-021-25875-y). Since college I have been fascinated with how the 3d structure of genome in the nucleus could influence gene expression. However, at that time little was known, and what was known was bogged down by jargon that made it incomprehensible to me. It was also around this time that [Hi-C](https://pubmed.ncbi.nlm.nih.gov/22652625/) was invented which revolutionized our ability to study the 3d conformation of the genome.

However, I never got a good answer to my thoughts until this paper. The authors start off by talking about Yamanaka factors, which are transcription factors that are able to induce pluripotency in terminally differentiated cell that shouldn't be able to change cell type. When Yamanaka's lab first did this, they only used 4 transcription factors. This revolutionized our understanding of cell biology. Prior to this study, the dogma was that cells could not dedifferentiate.

Around the same time Yamanaka was publishing this paper, [genome wide association studies](https://en.wikipedia.org/wiki/Genome-wide_association_study) were becoming a hot tool for trying to find genes responsible for phenotypes. Unfortunately, what they found were many genes influencing phenotypes. This created a conundrum - how could four genes completely change regulation and phenotype in a cell but so many genes be responsible for phenotypes at large? 

In this paper, the authors model a chromatain fiber as a chain of beads and a set of spheres as polymerase/transcription factors. The spheres can bind to specific beads on teh chromatin chain. These objects are then stuck in a brownian dynamics simulation to see how they interact. This simple and elegant approach is able to reproduce experimental results with regards to gene expression. It also reveals that GWAS can be explained by spatial effects alone while also showing that oversaturation of transcription factors can dramatically change the chromatin landscape. 

If this modeling works as well as this paper makes me think it does, this will create another revolution biology. Using these models we could predict how editing genes or regions of the genome will impact gene expression or what unintended consequences will occur due to remodeling of the chromatin. It also allows for testing hypotheses regarding gene regulation and phenotypes. My mind is absolutely blown by these implications and I am looking forward to trying my own hand at using this approach sometime in the near future. 


