---
title: "Raven Commandline Output"
permalink: /raven_output/
excerpt: ''
---

```bash
raven \
> -t 30 \
> /home/jon/Working_Files/sea_cuke_species_data/Chiridota_heheva/SRR15466781/SRR15466781.fastq \
> >raven_c_heheva.fasta
[raven::] loaded 2905301 sequences 570.482102s
[raven::Graph::Construct] minimized 0 - 246822 / 2905301 45.890172s
[raven::Graph::Construct] mapped sequences 51.263393s
[raven::Graph::Construct] minimized 246822 - 489359 / 2905301 43.817042s
[raven::Graph::Construct] mapped sequences 136.022934s
[raven::Graph::Construct] minimized 489359 - 822753 / 2905301 43.595922s
[raven::Graph::Construct] mapped sequences 211.224258s
[raven::Graph::Construct] minimized 822753 - 1176787 / 2905301 42.546025s
[raven::Graph::Construct] mapped sequences 393.462339s
[raven::Graph::Construct] minimized 1176787 - 1500892 / 2905301 40.541756s
[raven::Graph::Construct] mapped sequences 463.542068s
[raven::Graph::Construct] minimized 1500892 - 1809068 / 2905301 41.828002s
[raven::Graph::Construct] mapped sequences 575.048239s
[raven::Graph::Construct] minimized 1809068 - 2109452 / 2905301 41.833406s
[raven::Graph::Construct] mapped sequences 671.639439s
[raven::Graph::Construct] minimized 2109452 - 2437437 / 2905301 43.540541s
[raven::Graph::Construct] mapped sequences 771.669248s
[raven::Graph::Construct] minimized 2437437 - 2687587 / 2905301 49.687355s
[raven::Graph::Construct] mapped sequences 860.553370s
[raven::Graph::Construct] minimized 2687587 - 2905301 / 2905301 37.361492s
[raven::Graph::Construct] mapped sequences 856.598018s
[raven::Graph::Construct] annotated piles 4.788545s
[raven::Graph::Construct] removed contained sequences 20.887564s
[raven::Graph::Construct] removed chimeric sequences 79.100081s
[raven::Graph::Construct] reached checkpoint 23.551941s
[raven::Graph::Construct] minimized 0 - 23994 / 175841 62.121519s
[raven::Graph::Construct] mapped valid sequences 36.742298s
[raven::Graph::Construct] minimized 23994 - 47943 / 175841 72.305310s
[raven::Graph::Construct] mapped valid sequences 95.317262s
[raven::Graph::Construct] minimized 47943 - 84048 / 175841 68.193330s
[raven::Graph::Construct] mapped valid sequences 139.017650s
[raven::Graph::Construct] minimized 84048 - 113894 / 175841 65.449328s
[raven::Graph::Construct] mapped valid sequences 185.934661s
[raven::Graph::Construct] minimized 113894 - 142881 / 175841 66.222417s
[raven::Graph::Construct] mapped valid sequences 233.096907s
[raven::Graph::Construct] minimized 142881 - 170354 / 175841 67.679589s
[raven::Graph::Construct] mapped valid sequences 271.403012s
[raven::Graph::Construct] minimized 170354 - 175841 / 175841 22.449468s
[raven::Graph::Construct] mapped valid sequences 119.469741s
[raven::Graph::Construct] updated overlaps 2.155748s
[raven::Graph::Construct] removed false overlaps 9.387860s
[raven::Graph::Construct] stored 301076 nodes 34.161478s
[raven::Graph::Construct] stored 1095434 edges 1.056411s
[raven::Graph::Construct] reached checkpoint 47.512245s
[raven::Graph::Construct] 7149.669386s
[raven::Graph::Assemble] removed transitive edges 3.891697s
[raven::Graph::Assemble] reached checkpoint 43.920817s
[raven::Graph::Assemble] removed tips and bubbles 4670.174350s
[raven::Graph::Assemble] reached checkpoint 51.479616s
[raven::Graph::Assemble] removed long edges 1228.338644s
[raven::Graph::Assemble] reached checkpoint 39.528009s
[raven::Graph::Assemble] 6090.621186s
[racon::Polisher::Polish] minimized 0 - 2313 / 3227 64.202253s
[racon::Polisher::Polish] mapped sequences 1527.289548s
[racon::Polisher::Polish] minimized 2313 - 3227 / 3227 20.432926s
[racon::Polisher::Polish] mapped sequences 569.230824s
[racon::Polisher::Polish] found 2747806 overlaps 2184.284372s
[racon::Polisher::Polish] reverse complemented sequences 1.421397s
[racon::Polisher::Polish] aligned 2747806 / 2747806 overlaps [================] 3796.129271s
[racon::Polisher::Polish] prepared 2414083 window placeholders 97.781003s
[racon::Polisher::Polish] called consensus for 2414083 / 2414083 windows [================] 3269.628452s
[racon::Polisher::Polish] 9363.857152s
[raven::Graph::Polish] reached checkpoint 3.679862s
[racon::Polisher::Polish] minimized 0 - 2244 / 3227 60.165660s
[racon::Polisher::Polish] mapped sequences 1864.894727s
[racon::Polisher::Polish] minimized 2244 - 3227 / 3227 19.171942s
[racon::Polisher::Polish] mapped sequences 685.759432s
[racon::Polisher::Polish] found 2772633 overlaps 2633.102574s
[racon::Polisher::Polish] reverse complemented sequences 0.484145s
[racon::Polisher::Polish] aligned 2772633 / 2772633 overlaps [================] 2566.932142s
[racon::Polisher::Polish] prepared 2447059 window placeholders 86.542985s
[racon::Polisher::Polish] called consensus for 2447059 / 2447059 windows [================] 2539.672409s
[racon::Polisher::Polish] 7837.656129s
[raven::Graph::Polish] reached checkpoint 2.122253s
[raven::] 31083.016350s
```