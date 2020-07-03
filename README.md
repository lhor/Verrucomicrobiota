
# Guide to data, analysis, and results

This repository describes the data, analysis, and the results for the manuscript 'Marine *Verrucomicrobiota* populations degrade complex polysaccharides during phytoplankton blooms'.

### 1. Selection of representative Helgoland MAGs

All MAGs obtained from metagenomes during the spring blooms of 2010, 2011, 2012, and 2016 were previously uploaded and are available under the [PRJEB28156 study in ENA](https://www.ebi.ac.uk/ena/browser/view/PRJEB28156). A list with the names of the bins used ('.list') available in the folder '01.MAGs_from_Helgoland'.

##### i. MAG quality and selection
We first used [checkM](https://github.com/Ecogenomics/CheckM) to determine completeness and contamination among other stats. MAGs were selected based on completion and contamination according to the formula below:
```
%[completion] - 5*[%contamination] > 50
```

##### ii. Selection of representative MAGs
Pairwise genome-wide average nucleotide identities (ANI) values were determined using [fastANI](https://github.com/ParBLiSS/FastANI) for all MAGs with quality values above 50.

```
fastANI --ql q50-3300-MAGs.list --rl q50-3300-MAGs.list -o fastANI-3300_q50-PVC
```
The result was filtered using a identity value of 95% and an alignment fraction of 65% (`fastANI-3300_q50-PVC-65A`). Pairwise comparison were analyzed in Cytoscape and clusters were defined using edges of ANI values between 95% and 100% inclusive. Results are summarized in the file `MAGs.reps95ANI65AF.tsv`. Cluster representatives were selected based on the quality of each MAG:
```
quality=%[completion] - 5*[%contamination] +1/2 log10(N50)
```
Selected 444 MAGs and cluster numbers can be found in `MAGs.reps95ANI65AF.representatives.tsv`
