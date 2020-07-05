
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

### 2. Verrucomicrobiota MAGs from Helgoland

A total of 182 MAGs were classified as 'Verrucomicrobia' according to checkM and were selected for further analyses. A complete list is available in 'MAGs.reps95.Verrucomicrobiota.list'. A complete summary of all genome and taxonomic features for all 182 MAGs is available in 'MAGs.reps95.Verrucomicrobiota.xls'. To capture a higher intra-population diversity the MAGs within the *Verrucomicrobiota* group, all genomes were de-replicated at 99% ANI resulting in 26 representatives. The two additional MAGs c29 and c6 originated from the clusters c13 and c7. Thus, a total of 26 MAGs (ANI99) and 24 MAGs (ANI95) were determined depending on the ANI cutoff.

In order to improve the quality of the MAGs, anvio' was used to reduce contamination in some MAGs according to their [recommendations](http://merenlab.org/2017/01/03/loki-the-link-archaea-eukaryota/). The following *Verrucomicrobiota* cleaned and updated genomes were uploaded to the [PRJEB28156 study in ENA](https://www.ebi.ac.uk/ena/browser/view/PRJEB28156)

Gene predictions and initial gene annotations were determined using Prokka as folllow:
```
for mag in c3 c27 c26 c8 c9 c4 c5 c6 c7 c2 c10 c11 c12 c13 c14 c29 c15 c16 c17 c18 c19 c20 c21 c22 c23 c24; do
	prokka --cpus 8 --kingdom Bacteria --outdir ${mag} --locustag ${mag} --metagenome --prefix ${mag} ${mag}.fa;
done
```
A refined taxonomic determination was determined using [GTDB-tk](https://github.com/Ecogenomics/GTDBTk):
```
gtdbtk classify_wf -x fa --cpus 24 --genome_dir 00.MAGs/ --out_dir 01.GTDB-k/
```
### 3. MAG abundances

The fraction of MAGs in each metagenome (used as a proxy for abundance) were determined as the quotient between the truncated average sequencing depth (TAD80) of each MAG and the sequencing depth of the *rpoB* gene in each metagenome. The TAD80 was determined using the `BedGraph.tad.rb` script from the [enveomics collection](https://github.com/lmrodriguezr/enveomics).
The TAD80 was determined for all 444 representative MAGs in all metagenomes from 2010, 2011, 2012, and 2012. A summary of the commands used for each determination is available below, where the variables `$MAG` and `$MG` represent each MAG and metagenome respectively. TAD80 values for Verrucomicrobiota MAGs are available in "02.MAG_abundances/TAD80_Verrucomicrobiota.tsv".

```
bowtie2 --reorder --no-unal -f -p 16 -x ${MAG}.bwt2.db -1 ${MG}.1.fa -2 ${MG}.2.fa  > ${MAG}.${MG}.sam;
samtools view -bS -F 4 -bS ${MAG}.${MG}.sam | samtools sort - ${MAG}.${MG}.sorted;
bedtools genomecov -ibam ${MAG}.${MG}.sorted.bam -bga > ${MAG}.${MG}.sorted.bam.bg;
echo -e $MG "\t" $(BedGraph.tad.rb -i ${MAG}.${MG}.sorted.bam.bg -r 0.8);
rm ${MAG}.${MG}.sam;
```
The sequencing depth for rpoB genes was determined for each metagenome using a curated database previously published in [ROCker](https://github.com/lmrodriguezr/rocker) and BLASTx. Results were filtered using the corresponding ROCker filter. Filtered counts for rpoB are available in "02.MAG_abundances/rpoB_ROCker_Helgoland-metagenomes.tsv".

### 4. Phylogenies
The quality for 1,906 genomes and MAGs collected was determined using checkM `03.Verruco-collection/out_checkM_PVC_collect-1906.tab`. A total 1,827 genomes were selected based on their quality (Q50). Similarly to the process used for selecting *Verrucomicrobiota* MAGs from Helgoland, representatives were determined based on ANI 99 % and quality. A total of 1002 representatives were selected. Complete list available in `03.Verruco-collection/selected.reps.99ANI65AF.1002.list`.
A phylogenetic analysis of recovered MAGs and genomes belonging to *Verrucomicrobiota* and closely related genomes, was performed using the alignment of 120 conserved single copy genes generated by GTDB-tk:

```
FastTree -gamma -lg < gtdbtk.bac120.user_msa.fasta >PVC-collect-Helgoland.1024.v3.tree
```
The summary including *Verrucomicrobia* MAGs from Helgoland is available in `03.Verruco-collection/gtdbtk.bac120.summary.tsv`.

### 5. Gene annotations
### 6. Metaproteomic analyses
### 7. 16S rRNA gene oligotyping
### 8. Data availability
