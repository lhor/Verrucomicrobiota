
# Guide to data, analysis, and results

This repository describes the data, analysis, and the results for the manuscript 'Marine *Verrucomicrobiota* populations degrade complex polysaccharides during phytoplankton blooms'.

### 1. Selection of representative Helgoland MAGs

All MAGs obtained from metagenomes during the spring blooms of 2010, 2011, 2012, and 2016 were previously uploaded and are available under the [PRJEB28156 study in ENA](https://www.ebi.ac.uk/ena/browser/view/PRJEB28156). A list with the names of the bins used ('.list') available in the folder '01.MAGs_from_Helgoland'.

##### i. MAG quality and selection
We first used [checkM](https://github.com/Ecogenomics/CheckM) to determine completeness and contamination among other stats.

##### ii. Selection of representative MAGs

```
fastANI --ql q50-3300-MAGs.list --rl q50-3300-MAGs.list -o fastANI-3300_q50-PVC
```
