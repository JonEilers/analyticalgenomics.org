---
title: "Shasta Commandline Output"
permalink: /shasta_output/
excerpt: ''
---

```bash
./shasta \
> --threads=30 \
> --config /home/jon/Working_Files/shasta/conf/Nanopore-Oct2021.conf \
> --input /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
> --assemblyDirectory /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta \
> --memoryBacking 2M \
> --memoryMode filesystem
Shasta Release 0.8.0
2021-Oct-11 19:07:24.905011 Assembly begins with the following command line:
./shasta --threads=30 --config /home/jon/Working_Files/shasta/conf/Nanopore-Oct2021.conf --input /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq --assemblyDirectory /home/jon/Working_Files/Chiridota_heheva_genome_project/genome_assembly/shasta --memoryBacking 2M --memoryMode filesystem 
[sudo] password for jon: 
For options in use for this assembly, see shasta.conf in the assembly directory.
This assembly will use 30 threads.
Setting up consensus caller Bayesian:guppy-5.0.7-a
Using predefined Bayesian consensus caller guppy-5.0.7-a
Bayesian consensus caller configuration name is guppy5.0.7_hg002_chr1-2 with pseudocounts 1, Jun 1 2021
2021-Oct-11 19:07:27.897040 Begin loading reads from 1 files.
2021-Oct-11 19:07:27.897105 Loading reads from /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq
File size: 85103149040 bytes.
Buffer allocate time: 116.226 s.
Read time: 86.0956 s.
Read rate: 9.88473e+08 bytes/s.
Found 11621204 lines in this file.
Time to process this file:
Allocate buffer + read: 202.322 s.
Locate: 5.59126 s.
Parse: 39.7668 s.
Store: 46.8905 s.
Total: 294.571 s.
Discarded read statistics for file /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq:
    Discarded 0 reads containing invalid bases for a total 0 valid bases.
    Discarded 1358773 reads shorter than 10000 bases for a total 6921653005 bases.
    Discarded 0 reads containing repeat counts 256 or more for a total 0 bases.
    Discarded 0 reads with palindromic quality scores for a total 0 bases.
See ReadLengthHistogram.csv and Binned-ReadLengthHistogram.csv for details of the read length distribution.
Discarded read statistics for all input files:
    Discarded 0 reads containing invalid bases for a total 0 valid bases.
    Discarded 1358773 short reads for a total 6921653005 bases.
    Discarded 0 reads containing repeat counts 256 or more for a total 0 bases.
Read statistics for reads that will be used in this assembly:
    Total number of reads is 1546528.
    Total number of raw bases is 35508735054.
    Average read length is 22960.3 bases.
    N50 for read length is 26052 bases.
    The above statistics only include reads that will be used in this assembly.
    Read discarded because they contained invalid bases, were too short or contained repeat counts 256 or more are not counted.
2021-Oct-11 19:12:46.248794 Done loading reads from 1 files.
Read loading took 318.352s.
Selected 26839165 14-mers as markers out of 268435456 total.
Requested inclusion probability: 0.1.
Actual fraction of marker k-mers: 0.0999837.
The above statistics include all k-mers, not just those present in run-length encoded sequence.
2021-Oct-11 19:13:36.726188 Finding markers in 1546528 reads.
2021-Oct-11 19:15:23.888276 Finding markers completed in 107.162 s.
2021-Oct-11 19:15:23.888405 Finding palindromic reads.
2021-Oct-11 19:15:23.889875 0/1546528
2021-Oct-11 19:15:35.780816 1000000/1546528
2021-Oct-11 19:15:42.611000 Flagged 13202 reads as palindromic out of 1546528 total.
Palindromic fraction is 0.00853654
2021-Oct-11 19:15:42.612728 LowHash0 begins.
LowHash0 algorithm will use 2^31 = 2147483648 buckets. 
2021-Oct-11 19:15:42.612751 Creating kmer ids for oriented reads.
There are 1546528 reads, 3093056 oriented reads.
2021-Oct-11 19:15:58.099866 LowHash0 iteration 0 begins.
Alignment candidates after iteration 0: high frequency 122112, total 21137390, capacity 21137390.
2021-Oct-11 19:16:48.766980 LowHash0 iteration 1 begins.
Alignment candidates after iteration 1: high frequency 573229, total 38394214, capacity 46275612.
2021-Oct-11 19:17:27.321829 LowHash0 iteration 2 begins.
Alignment candidates after iteration 2: high frequency 1398879, total 53531125, capacity 70377315.
2021-Oct-11 19:18:05.268077 LowHash0 iteration 3 begins.
Alignment candidates after iteration 3: high frequency 2478370, total 67324306, capacity 89660498.
2021-Oct-11 19:18:42.579341 LowHash0 iteration 4 begins.
Alignment candidates after iteration 4: high frequency 3699890, total 80144678, capacity 107769775.
2021-Oct-11 19:19:18.979882 LowHash0 iteration 5 begins.
Alignment candidates after iteration 5: high frequency 4974077, total 92275892, capacity 125019243.
2021-Oct-11 19:19:56.602460 LowHash0 iteration 6 begins.
Alignment candidates after iteration 6: high frequency 6255245, total 103785386, capacity 141451867.
2021-Oct-11 19:20:37.815552 LowHash0 iteration 7 begins.
Alignment candidates after iteration 7: high frequency 7502801, total 114735784, capacity 157098354.
2021-Oct-11 19:21:16.955318 LowHash0 iteration 8 begins.
Alignment candidates after iteration 8: high frequency 8705629, total 125359304, capacity 172339691.
2021-Oct-11 19:21:55.579032 LowHash0 iteration 9 begins.
Alignment candidates after iteration 9: high frequency 9860039, total 135430364, capacity 186897156.
2021-Oct-11 19:22:35.944788 Storing candidate alignments.
Found 9860039 alignment candidates.
Average number of alignment candidates per oriented read is 6.3756.
2021-Oct-11 19:22:42.577360 LowHash0 completed in 419.965 s.
2021-Oct-11 19:22:43.971488 Suppressing alignment candidates.
Number of alignment candidates before suppression is 9860039
Suppressed 0 alignment candidates.
Number of alignment candidates after suppression is 9860039
2021-Oct-11 19:22:44.120815 Done suppressing alignment candidates.
2021-Oct-11 19:22:49.949694 Begin computing alignments for 9860039 alignment candidates.
2021-Oct-11 19:22:49.949787 Alignment computation begins.
2021-Oct-11 19:22:49.952655 Working on alignment 0 of 9860039
2021-Oct-11 19:23:41.154688 Working on alignment 1000000 of 9860039
2021-Oct-11 19:24:31.156591 Working on alignment 2000000 of 9860039
2021-Oct-11 19:25:21.083449 Working on alignment 3000000 of 9860039
2021-Oct-11 19:26:10.287791 Working on alignment 4000000 of 9860039
2021-Oct-11 19:26:50.950905 Working on alignment 5000000 of 9860039
2021-Oct-11 19:27:30.197443 Working on alignment 6000000 of 9860039
2021-Oct-11 19:28:13.486707 Working on alignment 7000000 of 9860039
2021-Oct-11 19:28:57.795995 Working on alignment 8000000 of 9860039
2021-Oct-11 19:29:42.930484 Working on alignment 9000000 of 9860039
2021-Oct-11 19:30:23.752975 Alignment computation completed.
2021-Oct-11 19:30:23.753059 Storing the alignment found by each thread.
Found and stored 7224067 good alignments.
2021-Oct-11 19:30:29.273114 Creating alignment table.
2021-Oct-11 19:30:33.336436 Computation of alignments completed in 463.387 s.
2021-Oct-11 19:30:33.336611 Selected thresholds automatically for the following parameters:
	alignedFraction:	0.235
	markerCount:		95
	maxDrift:		25.5
	maxSkip:		58.5
	maxTrim:		81.5
Keeping 3845774 alignments of 7224067
2021-Oct-11 19:30:40.176104 Begin flagCrossStrandReadGraphEdges.
2021-Oct-11 19:30:40.288921 20 0/1546528
2021-Oct-11 19:30:40.335819 7 100000/1546528
2021-Oct-11 19:30:40.340184 23 200000/1546528
2021-Oct-11 19:30:41.652901 28 300000/1546528
2021-Oct-11 19:30:42.057971 7 400000/1546528
2021-Oct-11 19:30:42.280511 16 500000/1546528
2021-Oct-11 19:30:42.873391 29 600000/1546528
2021-Oct-11 19:30:43.206424 12 700000/1546528
2021-Oct-11 19:30:43.854778 6 800000/1546528
2021-Oct-11 19:30:44.121408 24 900000/1546528
2021-Oct-11 19:30:44.859844 12 1000000/1546528
2021-Oct-11 19:30:45.463960 10 1100000/1546528
2021-Oct-11 19:30:45.690010 21 1200000/1546528
2021-Oct-11 19:30:46.540919 12 1300000/1546528
2021-Oct-11 19:30:46.880946 11 1400000/1546528
2021-Oct-11 19:30:47.051998 14 1500000/1546528
Of 3093056 vertices in the read graph, 142122 are within distance 6 of their reverse complement.
Found 2351 strand jump regions.
Marked 30064 read graph edges out of 7691548 total as cross-strand.
2021-Oct-11 19:30:49.050572 End flagCrossStrandReadGraphEdges.
2021-Oct-11 19:30:49.070470 Begin flagging chimeric reads, max distance 2
2021-Oct-11 19:30:49.070563 Processing 1546528 reads.
2021-Oct-11 19:30:50.990985 Done flagging chimeric reads.
2021-Oct-11 19:30:50.994296 Flagged 51041 reads as chimeric out of 1546528 total.
Chimera rate is 0.0330036
2021-Oct-11 19:30:50.999367 Computing connected components of the read graph.
The read graph has 1054250 connected components.
2021-Oct-11 19:30:52.962547 Done computing connected components of the read graph.
2021-Oct-11 19:30:53.229189 Begin computing marker graph vertices.
2021-Oct-11 19:32:13.063789 Disjoint set computation begins.
2021-Oct-11 19:32:32.116017 Disjoint set computation completed.
2021-Oct-11 19:32:32.116111 Finding the disjoint set that each oriented marker was assigned to.
    2021-Oct-11 19:32:32.116120  Iteration  1
    2021-Oct-11 19:33:33.347469  Updated parent of - 84219230 entries.
    2021-Oct-11 19:33:33.347559  Iteration  2
    2021-Oct-11 19:33:42.404092  Updated parent of - 541961 entries.
    2021-Oct-11 19:33:42.404202  Iteration  3
    2021-Oct-11 19:33:50.997982  Updated parent of - 0 entries.
2021-Oct-11 19:33:50.998068 Verifying convergence of parent information.
2021-Oct-11 19:33:59.872103 Done verifying convergence of parent information.
2021-Oct-11 19:33:59.872192 Compacting the Disjoint Set data-structure.
2021-Oct-11 19:34:33.295366 Done compacting the Disjoint Set data-structure.
2021-Oct-11 19:34:33.295469 Counting the number of markers in each disjoint set.
Processing 4930121196 oriented markers.
Unable to automatically select MarkerGraph.minCoverage. No significant cutoff found in disjoint sets size distribution. Observed peak has percent total area of 3.07499e-05
minPercentArea is 0.08
See DisjointSetsHistogram.csv.Using MarkerGraph.minCoverage = 5
2021-Oct-11 19:35:51.138172 Renumbering the disjoint sets.
Kept 123083224 disjoint sets with coverage in the requested range.
2021-Oct-11 19:36:21.119144 Assigning vertices to renumbered disjoint sets.
2021-Oct-11 19:37:06.144517 Gathering markers in disjoint sets, pass1.
2021-Oct-11 19:37:07.133188 Processing 4930121196 oriented markers.
2021-Oct-11 19:37:10.953843 Gathering markers in disjoint sets, pass2.
2021-Oct-11 19:37:19.260592 Processing 4930121196 oriented markers.
2021-Oct-11 19:37:35.200697 Sorting the markers in each disjoint set.
2021-Oct-11 19:37:36.699900 Flagging bad disjoint sets.
Found 3882710 disjoint sets with more than one marker on a single oriented read or with less than 0 supporting oriented reads on each strand.
2021-Oct-11 19:37:45.366247 Renumbering disjoint sets to remove the bad ones.
2021-Oct-11 19:37:46.405872 Assigning vertex ids to markers.
2021-Oct-11 19:38:36.819436 Gathering the markers of each vertex of the marker graph.
2021-Oct-11 19:39:18.741114 Computation of global marker graph vertices completed in 505.512 s.
2021-Oct-11 19:39:18.741276 Begin findMarkerGraphReverseComplementVertices.
2021-Oct-11 19:39:28.695381 Begin findMarkerGraphReverseComplementVertices.
2021-Oct-11 19:39:28.695484 createMarkerGraphEdges begins.
2021-Oct-11 19:39:28.695502 Processing 119200514 marker graph vertices.
2021-Oct-11 19:39:47.893562 Combining the edges found by each thread.
2021-Oct-11 19:40:28.219906 Found 312213408 edges for 119200514 vertices.
2021-Oct-11 19:40:38.729157 createMarkerGraphEdges ends.
2021-Oct-11 19:40:38.729256 Begin findMarkerGraphReverseComplementEdges.
2021-Oct-11 19:40:48.608545 End findMarkerGraphReverseComplementEdges.
2021-Oct-11 19:40:48.608691 Transitive reduction of the marker graph begins.
The marker graph has 119200514 vertices and 312213408 edges.
2021-Oct-11 19:41:01.130660 Flagged as weak 196000 edges with coverage 1 and marker skip greater than 100
2021-Oct-11 19:42:40.718881 Flagged as weak 129560748 edges with coverage 1 out of 132273196 total.
2021-Oct-11 19:43:24.634549 Flagged as weak 36846366 edges with coverage 2 out of 42290190 total.
2021-Oct-11 19:43:48.125447 Flagged as weak 13311674 edges with coverage 3 out of 24368328 total.
2021-Oct-11 19:44:03.374867 Flagged as weak 5219884 edges with coverage 4 out of 22211134 total.
2021-Oct-11 19:44:14.252956 Flagged as weak 2196956 edges with coverage 5 out of 25170250 total.
2021-Oct-11 19:44:20.979999 Flagged as weak 1004584 edges with coverage 6 out of 16958788 total.
2021-Oct-11 19:44:25.407984 Flagged as weak 482062 edges with coverage 7 out of 12233794 total.
2021-Oct-11 19:44:28.425366 Flagged as weak 242230 edges with coverage 8 out of 9051684 total.
2021-Oct-11 19:44:30.593162 Flagged as weak 125036 edges with coverage 9 out of 6792138 total.
2021-Oct-11 19:44:32.095946 Flagged as weak 67376 edges with coverage 10 out of 5140544 total.
2021-Oct-11 19:44:33.186423 Flagged as weak 37240 edges with coverage 11 out of 3915086 total.
2021-Oct-11 19:44:33.989011 Flagged as weak 21406 edges with coverage 12 out of 2996374 total.
2021-Oct-11 19:44:34.590849 Flagged as weak 13220 edges with coverage 13 out of 2284214 total.
2021-Oct-11 19:44:35.043486 Flagged as weak 8404 edges with coverage 14 out of 1726168 total.
2021-Oct-11 19:44:35.387524 Flagged as weak 5272 edges with coverage 15 out of 1292678 total.
2021-Oct-11 19:44:35.648781 Flagged as weak 3724 edges with coverage 16 out of 953054 total.
2021-Oct-11 19:44:35.848990 Flagged as weak 2866 edges with coverage 17 out of 694672 total.
2021-Oct-11 19:44:36.003025 Flagged as weak 2128 edges with coverage 18 out of 499874 total.
2021-Oct-11 19:44:36.119015 Flagged as weak 1632 edges with coverage 19 out of 354538 total.
2021-Oct-11 19:44:36.211833 Flagged as weak 1230 edges with coverage 20 out of 249668 total.
2021-Oct-11 19:44:36.284950 Flagged as weak 984 edges with coverage 21 out of 175330 total.
2021-Oct-11 19:44:36.340897 Flagged as weak 814 edges with coverage 22 out of 125018 total.
2021-Oct-11 19:44:36.385608 Flagged as weak 596 edges with coverage 23 out of 88698 total.
2021-Oct-11 19:44:36.422943 Flagged as weak 528 edges with coverage 24 out of 65014 total.
2021-Oct-11 19:44:36.453830 Flagged as weak 430 edges with coverage 25 out of 48524 total.
2021-Oct-11 19:44:36.480022 Flagged as weak 328 edges with coverage 26 out of 37948 total.
2021-Oct-11 19:44:36.503152 Flagged as weak 292 edges with coverage 27 out of 30120 total.
2021-Oct-11 19:44:36.522146 Flagged as weak 244 edges with coverage 28 out of 24640 total.
2021-Oct-11 19:44:36.538441 Flagged as weak 204 edges with coverage 29 out of 20490 total.
2021-Oct-11 19:44:36.552343 Flagged as weak 176 edges with coverage 30 out of 17048 total.
2021-Oct-11 19:44:36.563565 Flagged as weak 146 edges with coverage 31 out of 14764 total.
2021-Oct-11 19:44:36.573560 Flagged as weak 122 edges with coverage 32 out of 12864 total.
2021-Oct-11 19:44:36.581931 Flagged as weak 108 edges with coverage 33 out of 11122 total.
2021-Oct-11 19:44:36.588959 Flagged as weak 106 edges with coverage 34 out of 9776 total.
2021-Oct-11 19:44:36.596170 Flagged as weak 64 edges with coverage 35 out of 8644 total.
2021-Oct-11 19:44:36.601652 Flagged as weak 60 edges with coverage 36 out of 7104 total.
2021-Oct-11 19:44:36.606452 Flagged as weak 66 edges with coverage 37 out of 6346 total.
2021-Oct-11 19:44:36.610258 Flagged as weak 54 edges with coverage 38 out of 5628 total.
2021-Oct-11 19:44:36.614163 Flagged as weak 36 edges with coverage 39 out of 4984 total.
2021-Oct-11 19:44:36.617653 Flagged as weak 40 edges with coverage 40 out of 4516 total.
2021-Oct-11 19:44:36.620525 Flagged as weak 34 edges with coverage 41 out of 3882 total.
2021-Oct-11 19:44:36.622919 Flagged as weak 28 edges with coverage 42 out of 3566 total.
2021-Oct-11 19:44:36.625128 Flagged as weak 22 edges with coverage 43 out of 3276 total.
2021-Oct-11 19:44:36.627194 Flagged as weak 12 edges with coverage 44 out of 2772 total.
2021-Oct-11 19:44:36.629256 Flagged as weak 18 edges with coverage 45 out of 2522 total.
2021-Oct-11 19:44:36.630491 Flagged as weak 14 edges with coverage 46 out of 2140 total.
2021-Oct-11 19:44:36.631688 Flagged as weak 6 edges with coverage 47 out of 2030 total.
2021-Oct-11 19:44:36.633102 Flagged as weak 6 edges with coverage 48 out of 1900 total.
2021-Oct-11 19:44:36.634283 Flagged as weak 12 edges with coverage 49 out of 1574 total.
2021-Oct-11 19:44:36.635116 Flagged as weak 14 edges with coverage 50 out of 1440 total.
2021-Oct-11 19:44:36.635895 Flagged as weak 10 edges with coverage 51 out of 1278 total.
2021-Oct-11 19:44:36.636387 Flagged as weak 6 edges with coverage 52 out of 1202 total.
2021-Oct-11 19:44:36.637277 Flagged as weak 4 edges with coverage 53 out of 1124 total.
2021-Oct-11 19:44:36.638413 Flagged as weak 2 edges with coverage 55 out of 844 total.
2021-Oct-11 19:44:36.639011 Flagged as weak 6 edges with coverage 56 out of 820 total.
2021-Oct-11 19:44:36.639469 Flagged as weak 2 edges with coverage 57 out of 724 total.
Transitive reduction removed 189355632 marker graph edges out of 312213408 total.
The marker graph has 119200514 vertices and 122857776 strong edges.
2021-Oct-11 19:44:37.739648 Transitive reduction of the marker graph ends.
2021-Oct-11 19:44:39.441766 Begin prune iteration 0
Pruned 54088 edges at prune iteration 0.
2021-Oct-11 19:45:18.445324 Begin prune iteration 1
Pruned 52748 edges at prune iteration 1.
2021-Oct-11 19:45:57.436547 Begin prune iteration 2
Pruned 51998 edges at prune iteration 2.
2021-Oct-11 19:46:36.551224 Begin prune iteration 3
Pruned 51488 edges at prune iteration 3.
2021-Oct-11 19:47:15.467210 Begin prune iteration 4
Pruned 50976 edges at prune iteration 4.
2021-Oct-11 19:47:54.380788 Begin prune iteration 5
Pruned 50516 edges at prune iteration 5.
The original marker graph had 119200514 vertices and 312213408 edges.
The number of surviving edges is 122545962.
2021-Oct-11 19:48:43.459271 Begin simplifyMarkerGraph iteration 0 with maxLength = 10
Before iteration 0 part 1, the assembly graph has 6608288 vertices and 10265628 edges.
Before iteration 0 part 2, the assembly graph has 2206946 vertices and 3386984 edges.
2021-Oct-11 19:51:32.532346 Begin simplifyMarkerGraph iteration 1 with maxLength = 100
Before iteration 1 part 1, the assembly graph has 410104 vertices and 578426 edges.
Before iteration 1 part 2, the assembly graph has 161856 vertices and 199386 edges.
2021-Oct-11 19:53:23.956869 Begin simplifyMarkerGraph iteration 2 with maxLength = 1000
Before iteration 2 part 1, the assembly graph has 89606 vertices and 91436 edges.
Before iteration 2 part 2, the assembly graph has 71828 vertices and 64750 edges.
Found a self-complementary component with 160 vertices.
Found a self-complementary component with 302 vertices.
Skipped a self-complementary component with 160 vertices.
Skipped a self-complementary component with 302 vertices.
2021-Oct-11 19:55:10.388935 Begin simplifyMarkerGraph iteration 3 with maxLength = 10000
Before iteration 3 part 1, the assembly graph has 54840 vertices and 45660 edges.
Before iteration 3 part 2, the assembly graph has 51940 vertices and 41310 edges.
Found a self-complementary component with 472 vertices.
Found a self-complementary component with 72 vertices.
Found a self-complementary component with 24 vertices.
Found a self-complementary component with 70 vertices.
Found a self-complementary component with 922 vertices.
Found a self-complementary component with 24 vertices.
Found a self-complementary component with 28 vertices.
Found a self-complementary component with 8 vertices.
Found a self-complementary component with 8 vertices.
Found a self-complementary component with 196 vertices.
Found a self-complementary component with 10 vertices.
Skipped a self-complementary component with 472 vertices.
Skipped a self-complementary component with 72 vertices.
Skipped a self-complementary component with 24 vertices.
Skipped a self-complementary component with 70 vertices.
Skipped a self-complementary component with 922 vertices.
Skipped a self-complementary component with 24 vertices.
Skipped a self-complementary component with 28 vertices.
Skipped a self-complementary component with 8 vertices.
Skipped a self-complementary component with 8 vertices.
Skipped a self-complementary component with 196 vertices.
Skipped a self-complementary component with 10 vertices.
2021-Oct-11 19:56:52.099132 Begin simplifyMarkerGraph iteration 4 with maxLength = 100000
Before iteration 4 part 1, the assembly graph has 50132 vertices and 39116 edges.
Before iteration 4 part 2, the assembly graph has 50128 vertices and 39110 edges.
Found a self-complementary component with 498 vertices.
Found a self-complementary component with 24 vertices.
Found a self-complementary component with 70 vertices.
Found a self-complementary component with 28 vertices.
Found a self-complementary component with 94 vertices.
Found a self-complementary component with 938 vertices.
Found a self-complementary component with 28 vertices.
Found a self-complementary component with 8 vertices.
Found a self-complementary component with 8 vertices.
Found a self-complementary component with 204 vertices.
Found a self-complementary component with 12 vertices.
Found a self-complementary component with 16 vertices.
Skipped a self-complementary component with 498 vertices.
Skipped a self-complementary component with 24 vertices.
Skipped a self-complementary component with 70 vertices.
Skipped a self-complementary component with 28 vertices.
Skipped a self-complementary component with 94 vertices.
Skipped a self-complementary component with 938 vertices.
Skipped a self-complementary component with 28 vertices.
Skipped a self-complementary component with 8 vertices.
Skipped a self-complementary component with 8 vertices.
Skipped a self-complementary component with 204 vertices.
Skipped a self-complementary component with 12 vertices.
Skipped a self-complementary component with 16 vertices.
Removed 3018 low coverage cross edges of the assembly graph and 7254 corresponding marker graph edges.
2021-Oct-11 20:00:19.482356 Before detangling, the assembly graph has 45014 vertices and 30978 edges.
2021-Oct-11 20:00:19.502040 Filling in oriented reads.
2021-Oct-11 20:00:47.915466 Creating the tangles.
Found 1008 tangles.
The detangled assembly graph has 44558 vertices.
The detangled assembly graph has 30294 edges.
Removed 232 low coverage cross edges of the assembly graph and 2910 corresponding marker graph edges.
2021-Oct-11 20:01:59.679509 assembleMarkerGraphVertices begins.
2021-Oct-11 20:04:21.171365 assembleMarkerGraphVertices ends.
2021-Oct-11 20:04:21.171488 assembleMarkerGraphEdges begins.
2021-Oct-11 20:04:21.176614 0/312213408
2021-Oct-11 20:04:24.469282 10000000/312213408
2021-Oct-11 20:04:26.703129 20000000/312213408
2021-Oct-11 20:04:28.908837 30000000/312213408
2021-Oct-11 20:04:31.076704 40000000/312213408
2021-Oct-11 20:04:33.225712 50000000/312213408
2021-Oct-11 20:04:35.286521 60000000/312213408
2021-Oct-11 20:04:37.364055 70000000/312213408
2021-Oct-11 20:04:39.440402 80000000/312213408
2021-Oct-11 20:04:41.482963 90000000/312213408
2021-Oct-11 20:04:43.551072 100000000/312213408
2021-Oct-11 20:04:45.581773 110000000/312213408
2021-Oct-11 20:04:47.610581 120000000/312213408
2021-Oct-11 20:04:49.698242 130000000/312213408
2021-Oct-11 20:04:51.761313 140000000/312213408
2021-Oct-11 20:04:53.869536 150000000/312213408
2021-Oct-11 20:04:55.910747 160000000/312213408
2021-Oct-11 20:04:57.971920 170000000/312213408
2021-Oct-11 20:05:00.046323 180000000/312213408
2021-Oct-11 20:05:02.087489 190000000/312213408
2021-Oct-11 20:05:04.119275 200000000/312213408
2021-Oct-11 20:05:06.165821 210000000/312213408
2021-Oct-11 20:05:08.219112 220000000/312213408
2021-Oct-11 20:05:10.267101 230000000/312213408
2021-Oct-11 20:05:12.320278 240000000/312213408
2021-Oct-11 20:05:14.367635 250000000/312213408
2021-Oct-11 20:05:16.376011 260000000/312213408
2021-Oct-11 20:05:18.393194 270000000/312213408
2021-Oct-11 20:05:20.442657 280000000/312213408
2021-Oct-11 20:05:22.473648 290000000/312213408
2021-Oct-11 20:05:24.491611 300000000/312213408
2021-Oct-11 20:05:26.483511 310000000/312213408
2021-Oct-11 20:05:52.557838 assembleMarkerGraphEdges ends.
Using 30 threads.
Assembly begins for 30398 edges of the assembly graph.
2021-Oct-11 20:06:14.379625 Assembled a total 1209883028 bases for 30398 assembly graph edges of which 15199 were assembled.
The assembly graph has 44670 vertices and 30398 edges of which 15199 were assembled.
Total length of assembled sequence is 1209883028
N50 for assembly segments is 201222
2021-Oct-11 20:06:14.776341 writeGfa1 begins
2021-Oct-11 20:06:57.971307 writeGfa1 ends
2021-Oct-11 20:06:57.971426 writeGfa1BothStrands begins
2021-Oct-11 20:08:25.223429 writeGfa1BothStrands ends
2021-Oct-11 20:08:25.239070 writeFasta begins
2021-Oct-11 20:09:08.517447 writeFasta ends
2021-Oct-11 20:11:01.060057 
Assembly time statistics:
    Elapsed seconds: 3700.62
    Elapsed minutes: 61.677
    Elapsed hours:   1.02795
Average CPU utilization: 0.144744
Peak Memory usage (bytes): 170085445632
Shasta Release 0.8.0


```