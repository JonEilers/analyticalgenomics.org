
## RepeatModeler2 and RepeatMasker
Identifying repetetive elements in genome assemblies

### Background
Excerpt from repeatmasker [website](http://repeatmasker.org/)

RepeatMasker is a program that screens DNA sequences for interspersed repeats and low complexity DNA sequences. The output of the program is a detailed annotation of the repeats that are present in the query sequence as well as a modified version of the query sequence in which all the annotated repeats have been masked (default: replaced by Ns). Currently over 56% of human genomic sequence is identified and masked by the program. Sequence comparisons in RepeatMasker are performed by one of several popular search engines including nhmmer, cross_match, ABBlast/WUBlast, RMBlast and Decypher. RepeatMasker makes use of curated libraries of repeats and currently supports Dfam ( profile HMM library derived from Repbase sequences ) and Repbase, a service of the Genetic Information Research Institute.

Excerpt from repeatmodeler [website](http://repeatmasker.org/RepeatModeler/)

RepeatModeler is a de novo transposable element (TE) family identification and modeling package. At the heart of RepeatModeler are three de-novo repeat finding programs ( RECON, RepeatScout and LtrHarvest/Ltr_retriever ) which employ complementary computational methods for identifying repeat element boundaries and family relationships from sequence data.

RepeatModeler assists in automating the runs of the various algorithms given a genomic database, clustering redundant results, refining and classifying the families and producing a high quality library of TE families suitable for use with RepeatMasker and ultimately for submission to the Dfam database (http://dfam.org).

### Installation
Installing repeatmodeler and repeatmasker is a pain. There is no conda packages. I followed the instructions on the [website](http://repeatmasker.org/RMDownload.html) for repeatmasker and for [repeatmodeler](http://repeatmasker.org/RepeatModeler/)

### Commands

repeatmodeler commands for platanus-allee assembly
```bash
./RepeatModeler-2.0/BuildDatabase -name P_cali ../assembly/out_consensusScaffold.fa
./RepeatModeler-2.0/RepeatModeler -database P_cali -pa 20 -LTRStruct 2> repeatmodelerstderr.log
```

repeatmodeler commands for redundans assembly
```bash
./RepeatModeler-2.0/BuildDatabase -name P_cali_redundans_ajap ../../redundans-pcali-ajap_assembly.fa

./RepeatModeler-2.0/RepeatModeler -database P_cali_redundans_ajap -pa 20 -LTRStruct 2> redun_pcali_ajap_repeatmodelerstderr.log
```

repeatmasker command for the redundans assembly
```bash
./RepeatMasker-4.1.0/RepeatMasker/RepeatMasker  -e rmblast -pa 20 -s -lib /home/jon/Working_Files/Patanus-Allee/repeats/RM_48126.MonDec20802362019/consensi.fa.classified -dir repeatmasking -xsmall -gff -u -excln -poly /home/jon/Working_Files/pcali_ajap.redundans.platanus.fa
```

### Results

The histogram from the redundans repeatmodeler create database run
```bash
Search Engine = rmblast
LTR Structural Analysis: Enabled
Random Number Seed: 1580350821
Database = P_cali_redundans_ajap ...............................
  - Sequences = 304189
  - Bases = 878550047
  - N50 = 232861
  - Contig Histogram:
  Size(bp)                                                        Count
  -----------------------------------------------------------------------
  2198101-2355094 |                                                   [  ]
  2041108-2198100 |                                                   [  ]
  1884115-2041107 |                                                   [ 2 ]
  1727122-1884114 |                                                   [  ]
  1570129-1727121 |                                                   [ 1 ]
  1413136-1570128 |                                                   [ 2 ]
  1256143-1413135 |                                                   [ 6 ]
  1099150-1256142 |                                                   [ 9 ]
  942157-1099149  |                                                   [ 20 ]
  785164-942156   |                                                   [ 53 ]
  628171-785163   |                                                   [ 88 ]
  471178-628170   |                                                   [ 195 ]
  314185-471177   |                                                   [ 294 ]
  157192-314184   |                                                   [ 606 ]
  200-157192      |************************************************** [ 302912 ]
```


repeatmasker results
```bash
==================================================
file name: pcali_ajap.redundans.platanus.fa
sequences:        304189
total length:  878550047 bp  (656513553 bp excl N/X-runs)
GC level:         37.23 %
bases masked:  170484507 bp ( 25.97 %)
==================================================
               number of      length   percentage
               elements*    occupied  of sequence
--------------------------------------------------
Retroelements        93550     18961232 bp    2.89 %
   SINEs:            14931      2587308 bp    0.39 %
   Penelope          21742      3384211 bp    0.52 %
   LINEs:            68166     13599431 bp    2.07 %
    CRE/SLACS            0            0 bp    0.00 %
     L2/CR1/Rex      31936      6728661 bp    1.02 %
     R1/LOA/Jockey     322        88846 bp    0.01 %
     R2/R4/NeSL          0            0 bp    0.00 %
     RTE/Bov-B        2917       696261 bp    0.11 %
     L1/CIN4          2596       321436 bp    0.05 %
   LTR elements:     10453      2774493 bp    0.42 %
     BEL/Pao           657       165151 bp    0.03 %
     Ty1/Copia         842       111629 bp    0.02 %
     Gypsy/DIRS1      6476      1957237 bp    0.30 %
       Retroviral       71        15271 bp    0.00 %

DNA transposons      70184     11625161 bp    1.77 %
   hobo-Activator    29601      4683085 bp    0.71 %
   Tc1-IS630-Pogo      495        94042 bp    0.01 %
   En-Spm                0            0 bp    0.00 %
   MuDR-IS905            0            0 bp    0.00 %
   PiggyBac            319        86798 bp    0.01 %
   Tourist/Harbinger  3936       623059 bp    0.09 %
   Other (Mirage,      839       125122 bp    0.02 %
    P-element, Transib)

Rolling-circles       1408       343783 bp    0.05 %

Unclassified:       691585    126184917 bp   19.22 %

Total interspersed repeats:   156771310 bp   23.88 %


Small RNA:           13173      1863810 bp    0.28 %

Satellites:           1718       201586 bp    0.03 %
Simple repeats:     209360      9472367 bp    1.44 %
Low complexity:      38909      1831651 bp    0.28 %
==================================================

* most repeats fragmented by insertions or deletions
  have been counted as one element
  Runs of >=20 X/Ns in query were excluded in % calcs


RepeatMasker Combined Database: Dfam_3.1
                                         
run with rmblastn version 2.9.0+
The query was compared to classified sequences in ".../consensi.fa.classified"
```

> I may need to rerun repeatmodeler/repeatmasker as I might not have used the best repeatmodeler repeat models. Also, ltrharvest was not successfully used in the repeatmodeler step. 