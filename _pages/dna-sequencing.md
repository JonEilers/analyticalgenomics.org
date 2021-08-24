---
title: "Genome Sequencing"
permalink: /dnasequencing/
toc: true
toc_label: "Genome Sequencing"
toc_sticky: true
author_profile: false
header:
  overlay_image: /assets/images/home/cuke1.jpg

---

There are several different technologies available for genome sequencing. For my research I used Illumina's Hiseq X platform for generating short reads and Oxford Nanopore Technologies' MinION for long read sequencing.


# Illumina

Below is an excerpt from Wikipedia regarding Illumina's sequencing by synthesis technique. 

![](/assets/images/resource_images/genome_sequencing/illumina.png){: .align-left width="400px" height="400px"}  "In this method, DNA molecules and primers are first attached on a slide or flow cell and amplified with polymerase so that local clonal DNA colonies, later coined "DNA clusters", are formed. To determine the sequence, four types of reversible terminator bases (RT-bases) are added and non-incorporated nucleotides are washed away. A camera takes images of the fluorescently labeled nucleotides. Then the dye, along with the terminal 3' blocker, is chemically removed from the DNA, allowing for the next cycle to begin. Unlike pyrosequencing, the DNA chains are extended one nucleotide at a time and image acquisition can be performed at a delayed moment, allowing for very large arrays of DNA colonies to be captured by sequential images taken from a single camera. " [Wikipedia](https://en.wikipedia.org/wiki/DNA_sequencing#High-throughput_methods)

Additionally, Illumina has a pdf [here](https://sapac.illumina.com/content/dam/illumina-marketing/documents/products/illumina_sequencing_introduction.pdf) that contains a lot of info on how the technologies works and its history. There is also this [video](https://www.youtube.com/watch?v=fCd6B5HRaZ8).

# Oxford Nanopore Technologies

ONT uses protein nanopores embedded in a membrane. See below for a wikipedia excerpt on how it works.

![](/assets/images/resource_images/genome_sequencing/nanopore_dna.jpeg){: .align-right width="400px" height="400px"}  "The biological or solid-state membrane, where the nanopore is found, is surrounded by electrolyte solution. The membrane splits the solution into two chambers. A bias voltage is applied across the membrane inducing an electric field that drives charged particles, in this case the ions, into motion. This effect is known as electrophoresis. For high enough concentrations, the electrolyte solution is well distributed and all the voltage drop concentrates near and inside the nanopore. This means charged particles in the solution only feel a force from the electric field when they are near the pore region. This region is often referred as the capture region. Inside the capture region, ions have a directed motion that can be recorded as a steady ionic current by placing electrodes near the membrane. Imagine now a nano-sized polymer such as DNA or protein placed in one of the chambers. This molecule also has a net charge that feels a force from the electric field when it is found in the capture region.The molecule approaches this capture region aided by brownian motion and any attraction it might have to the surface of the membrane. Once inside the nanopore, the molecule translocates through via a combination of electro-phoretic, electro-osmotic and sometimes thermo-phoretic forces. Inside the pore the molecule occupies a volume that partially restricts the flow of ions, observed as an ionic current drop. Based on various factors such as geometry, size and chemical composition, the change in magnitude of the ionic current and the duration of the translocation will vary. Different molecules then can then be sensed and potentially identified based on this modulation in ionic current. 

The magnitude of the electric current density across a nanopore surface depends on the nanopore's dimensions and the composition of DNA or RNA that is occupying the nanopore. Sequencing was made possible because, passing through the channel of the nanopore, the samples cause characteristic changes in the density of the electric current flowing through the nanopore. The total charge flowing through a nanopore channel is equal to the surface integral of electric current density flux across the nanopore unit normal surfaces between times t1 and t2." [Wikipedia](https://en.wikipedia.org/wiki/Nanopore_sequencing#Biological) 

If you want better info, their [website](https://nanoporetech.com/how-it-works) has some good videos on how it works
