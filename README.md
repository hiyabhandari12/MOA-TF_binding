# NAM Maize TF Binding, Chromatin Accessibility, and Gene Expression

## Contents

- [Data](#data)
- [Analysis](#analysis)
  1. TF × accessibility × expression correlation
  2. Drought-response analysis
  3. TF motif database and motif–RPKM correlation
  4. Genotype clustering
  5. Motif enrichment and discovery across clusters
  6. Downstream gene assignment

## Data

| Source | Details |
|---|---|
| Reference genome/annotation | B73v5 & Mo17 (for DAP-seq data)|
| RNA-seq | [SRA accessions](https://www.ncbi.nlm.nih.gov/sra?LinkName=bioproject_sra_all&from_uid=1101486) , WW + DS conditions, 69 runs each (23 lines × 3 reps); replicate-averaged RPKM matrix |
| ChIP-seq peaks | 104 TFs [Tu et al.](https://iastate.app.box.com/v/maizegdb-public/folder/362580095877) |
| DAP-seq peaks | 105 TFs (B73) + 191 TFs (Mo17, assembly GCA_022117705.1); [Data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE275897)
| MOA-seq | [Data](https://www.nature.com/articles/s41588-025-02246-7#data-availability) ; [Read Depth](https://drive.google.com/drive/folders/1sJDruZQT9sw0Gp858gIh8lgTiUO90yIw)

## Analysis

### 1. TF × accessibility × expression correlation

**ChIP-seq × MOA-seq × RPKM**

Files used: ChIP-seq peak BED (104 TFs), MOA-seq peaks/read depth per NAM genotype, RPKM matrix.

NAM×NAM bp-overlap consistency: bp overlap mean = 0.75, binary overlap mean = 0.63

| Method | Mean \|r\| |
|---|---|
| 1. NAM×NAM bp-overlap/binary overlap consistency | 0.75 / 0.63 | 
| 2. Summed bp overlap vs. RPKM | 0.134 |
| 3. Binary ≥50% overlap vs. RPKM | 0.141 |
| 4. MOA-seq peak presence vs. RPKM | 0.145 |
| 5. Central-window vs. RPKM | 0.150 / 0.152 / 0.151 |


**DAP-seq × MOA-seq × RPKM**

Files used: DAP-seq peak BED (105 TFs - B73), same MOA-seq/RPKM data as above.
| Method | Mean \|r\| |
|---|---|
|1. NAM×NAM bp-overlap consistency| 0.57|
|2. MOA–RPKM correlation (summed bp overlap)| 0.151|


### 2. Drought-response analysis 
Files used: MOA-seq bQTL "GenoOnly" read-depth set (25 genotypes, WW+DS), ChIP-seq union peak BED (104 TFs), WW and DS RPKM matrices built using FASTQ data from SRAs.

Mean |r| = 0.175 at the population level 

### 3. TF motif and motif–RPKM correlation
Files used: combined TF motif database (104 ChIP-seq motifs), MOA-seq regions, RPKM matrix. 

72 significant TF motifs (found using HOMER)tested (spanning both ChIP-seq- and DAP-seq-derived TFs). Mean |r| = 0.172 (read depth), 0.158 (peak presence/absence).


### 4. Genotype clustering (2MP dataset)
Files used: 25 NAM MOA-seq genotype files 

226,627 unique binary patterns found across 813,590 shared positions; dominant patterns were all-1s (~27%) and all-0s, removed before clustering (571K variable positions remained). k-modes at k=2000 produced 2,002 clusters, further generated BED files for each cluster.

### 5. Motif enrichment and discovery across clusters
Files used: 2,002 cluster BED files (400 motifs: 104 ChIP-seq + 105 B73 DAP-seq + 191 Mo17 DAP-seq).

De novo discovery: 44,440 motifs found, filtered down (q < 0.05 & ≥50% target) to 48 plant-TF motifs. Cross-referencing against known motifs: 38 with no known TF hit.

### 6. Downstream gene assignment
Files used: B73v5 gene BED file (converted from GFF3, 39,756 genes), the 38 novel-cluster BED files from section 5.

1,218 unique gene-cluster associations within 10kb across the 38 novel clusters.