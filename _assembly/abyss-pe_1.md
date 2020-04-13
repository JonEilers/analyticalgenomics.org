---
title: "Abyss-pe Assembly"
toc: true
toc_sticky: true
layout: single
---

## Abyss-pe 
Assemble the sea cuke genome with k-mer set to 96

### Background

Excerpt from the Abyss-pe [website](https://www.bcgsc.ca/resources/software/abyss)

ABySS is a de novo, parallel, paired-end sequence assembler that is designed for short reads. The single-processor version is useful for assembling genomes up to 100 Mbases in size. The parallel version is implemented using MPI and is capable of assembling larger genomes.

More information can be found on their [github page](https://github.com/bcgsc/abyss)

### Installation
There is no conda package for abyss. The abyss github page provides the following command for installing it. This utilizes the apt-get package management system provided by default in the ubuntu operating system

command
```
sudo apt-get install abyss
```

this installed version 2.0.2-3 which was released in 2016
```
apt-show-versions abyss

#result
abyss:amd64/bionic 2.0.2-3 
```

### Command   
Initially k-mer was set to 101, but abyss threw an error and said 96 was largest it found. Not sure what was happening there, but setting k to 96 worked.

```
abyss-pe k=96 j=10 name=P_cali in='P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq'
```

### Results  

n        |n:500   |L50    |min  |N80   |N50   |N20   |E-size  |max     |sum      |name
---      |---     |---    |---  |---   |---   |---   |---     |---     |---      |---
2208438  |433641  |99501  |500  |888   |1738  |3605  |2546    |53135   |614e6    |P_cali-unitigs.fa
1879070  |319993  |54010  |500  |1551  |3857  |7341  |4872    |53501   |699.7e6  |P_cali-contigs.fa
1851769  |296536  |42788  |500  |1667  |4703  |9400  |6185    |103673  |698.9e6  |P_cali-scaffolds.fa

Values are for the haploid genome. 