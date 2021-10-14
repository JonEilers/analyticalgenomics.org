---
title: "Genome Polishing"
permalink: /genome_polishing/
excerpt: "Arts and sciences are not cast in a mould, but are formed and perfected by degrees, by often handling and polishing, as bears leisurely lick their cubs into form. - Michel De Montaigne"

---

Genome assembly algorithms are not perfect. Especially long read genome assemblies that have noisy data. Insertions and deletions (aka indels) and adapter contamination can sneak their way into the genome assembly. This can be problem as it can mess up de novo gene prediction resulting in truncated genes or shifted reading frames. If the naive molecular geneticist then downloads these genes and tries to use them for wet lab research they'll find themselves very frustrated and confused. 

But there is an easy way to deal with this problem. It's called genome assembly polishing. 