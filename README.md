# Maize TF Binding, Chromatin Accessibility, and Gene Expression

## Contents

- [Data](#data)
- [Analysis](#analysis)
  1. [TF × accessibility × expression correlation](#analysis-1)
     - [ChIP-seq × MOA-seq × RPKM](#analysis-1-chip-seq)
     - [DAP-seq × MOA-seq × RPKM](#analysis-1-dap-seq)
  2. [Drought-response analysis](#analysis-2)
  3. [TF motif database and motif–RPKM correlation](#analysis-3)
  4. [Genotype clustering](#analysis-4)
  5. [Motif enrichment and discovery across clusters](#analysis-5)
  6. [Downstream gene assignment](#analysis-6)

## Data

| Source                       | Details |
|-------------------------------|---------|
| Reference genome/annotation  | B73v5 & Mo17 (for DAP-seq data) |
| RNA-seq                      | [SRA accessions](https://www.ncbi.nlm.nih.gov/sra?LinkName=bioproject_sra_all&from_uid=1101486), WW + DS conditions, 69 runs each (23 lines × 3 reps); replicate-averaged RPKM matrix |
| ChIP-seq peaks                | 104 TFs, [Tu et al.](https://iastate.app.box.com/v/maizegdb-public/folder/362580095877) |
| DAP-seq peaks                 | 105 TFs (B73) + 191 TFs (Mo17, assembly GCA_022117705.1); [Data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE275897) |
| MOA-seq                       | [Engelhorn et al.](https://www.nature.com/articles/s41588-025-02246-7#data-availability); [Read Depth Data](https://drive.google.com/drive/folders/1sJDruZQT9sw0Gp858gIh8lgTiUO90yIw) |

## Analysis

<a id="analysis-1"></a>
### 1. TF × accessibility × expression correlation

<a id="analysis-1-chip-seq"></a>
#### ChIP-seq × MOA-seq × RPKM

Files used: ChIP-seq peak BED (104 TFs), MOA-seq peaks/read depth per NAM genotype, RPKM matrix.

| Method                                              | Mean \|r\|    |
|------------------------------------------------------|---------------|
| 1. NAM×NAM bp-overlap / binary-overlap consistency    | 0.75 / 0.63   |
| 2. Summed bp overlap vs. RPKM                         | 0.134         |
| 3. Binary ≥50% overlap vs. RPKM                       | 0.141         |
| 4. MOA-seq peak presence vs. RPKM                     | 0.145         |
| 5. Central-window vs. RPKM                            | 0.150 / 0.152 / 0.151 |

**Method notes:**
1. bp-overlap value (0.75): pairwise Pearson R of summed bp-overlap across genotype pairs, averaged per TF then across 104 TFs. Binary-overlap value (0.63): same pairwise design on a binarized peak-presence matrix, but using squared Pearson R (R²) averaged per TF and across TFs
2. Summed bp overlap in each of the NAM lines and correlated against the RPKM
3. Per TF, Pearson-correlates a ≥50%-overlap-thresholded peak count against RPKM across genotypes.
4. A single genome-wide peak-presence (summed peak count per genotype, independent of TF identity), Spearman-correlated against each TF's own RPKM across genotypes.
5. Summed-bp-overlap-vs-RPKM design as row 2, restricted to a fixed window (20/50/100bp respectively) centered on each ChIP peak; Spearman, per TF across genotypes.

<a id="analysis-1-dap-seq"></a>
#### DAP-seq × MOA-seq × RPKM

Files used: DAP-seq peak BED (105 TFs - B73), same MOA-seq/RPKM data as above.

| Method                                    | Mean \|r\| |
|--------------------------------------------|------------|
| 1. NAM×NAM bp-overlap consistency          | 0.57       |
| 2. MOA–RPKM correlation (summed bp overlap)| 0.151      |

**Method notes:**
1. Pairwise-Pearson-R design as the ChIP-seq bp-overlap value, on the DAP bp-overlap matrix, averaged across 105 TFs.
2. Per TF, summed bp overlap vs. RPKM, Spearman across genotypes.

<a id="analysis-2"></a>
### 2. Drought-response analysis

**Files used:** MOA-seq read-depth set (25 genotypes, WW+DS), ChIP-seq union peak BED (104 TFs), WW and DS RPKM matrices built using FASTQ data from SRAs.

**Method:** Per TF, per genotype, computed delta_MOA = log2FC(DS vs WW) of summed MOA read depth at that TF's bQTLs, and delta_RPKM = log2FC(DS vs WW) of RPKM. Within each TF (n≥3 genotypes), computed the Spearman correlation between delta_MOA and delta_RPKM across genotypes, then averaged |r| across the 96 testable TFs — testing whether drought-induced changes in binding track drought-induced changes in expression, rather than static levels.

**Result:** Mean |r| = 0.175 

<a id="analysis-3"></a>
### 3. TF motif and motif–RPKM correlation

**Files used:** TF motif database (104 ChIP-seq motifs), MOA-seq regions, RPKM matrix.

**Result:** 72 significant TF motifs (found using HOMER) tested, spanning ChIP-seq derived TF motifs.

| Method                          | Mean \|r\| |
|----------------------------------|------------|
| 1. RPKM vs. Read depth                       | 0.172      |
| 2. RPKM vs. Peak presence/absence            | 0.158      |

**Method notes:**
1. For each of the significant motifs, averages MOA-seq read depth across positions falling inside that motif's HOMER-called peak regions per genotype, then Spearman-correlates against RPKM across 21 shared genotypes.
2. Same design, using binary peak presence/absence (instead of continuous read depth) at those positions.

<a id="analysis-4"></a>
### 4. Genotype clustering (2MP dataset)

**Files used:** 25 NAM MOA-seq genotype files.

**Result:**
- 226,627 unique binary patterns found across 813,590 shared positions
- Dominant patterns were all-1s and all-0s, removed before clustering (571K variable positions remained)
- k-modes at k=2000 produced 2,002 clusters; BED files generated for each cluster

<a id="analysis-5"></a>
### 5. Motif enrichment and discovery across clusters

**Files used:** 2,002 cluster BED files (400 motifs: 104 ChIP-seq + 105 B73 DAP-seq + 191 Mo17 DAP-seq).

**Result:**
- De novo discovery: 44,440 motifs found
- Filtered (q < 0.05 & ≥50% target) down to 48 plant-TF motifs
- Cross-referenced against known motifs: 38 with no known TF hit

<a id="analysis-6"></a>
### 6. Downstream gene assignment

**Files used:** B73v5 gene BED file (converted from GFF3, 39,756 genes), the 38 novel-cluster BED files from section 5.

**Result:** 1,218 unique gene-cluster associations within 10kb across the 38 novel clusters.
