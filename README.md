# NAM Maize TF Binding, Chromatin Accessibility, and Gene Expression

Integrating RNA-seq, ChIP-seq, DAP-seq, and MOA-seq (chromatin accessibility) across the maize NAM panel to test whether transcription factor binding and chromatin accessibility relate to gene expression, under well-watered and drought conditions.

## Contents

- [Data](#data)
- [Analysis](#analysis)
  1. [TF × accessibility × expression correlation](#1-tf--accessibility--expression-correlation)
  2. [Drought-response analysis](#2-drought-response-analysis-Δmoa-vs-Δrpkm)
  3. [TF motif database and motif–RPKM correlation](#3-tf-motif-database-and-motif–rpkm-correlation)
  4. [Genotype clustering](#4-genotype-clustering-2mp-dataset)
  5. [Motif enrichment and discovery across clusters](#5-motif-enrichment-and-discovery-across-clusters)
  6. [Downstream gene assignment](#6-downstream-gene-assignment)

## Data

| Source | Details |
|---|---|
| Reference genome/annotation | B73v5 & Mo17 (for DAP-seq data)|
| RNA-seq | SRA accessions [SRA accessions](https://www.ncbi.nlm.nih.gov/sra?LinkName=bioproject_sra_all&from_uid=1101486) , B73×W22 NAM lines, WW + DS conditions, 69 runs each (23 lines × 3 reps); replicate-averaged RPKM matrix |
| ChIP-seq peaks | 104 TFs [Tu et al.](https://iastate.app.box.com/v/maizegdb-public/folder/362580095877) |
| DAP-seq peaks | 105 TFs (B73) + 191 TFs (Mo17, assembly GCA_022117705.1) | https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE275897 |
| MOA-seq | [Data](https://www.nature.com/articles/s41588-025-02246-7#data-availability)|[Read Depth](https://drive.google.com/drive/folders/1sJDruZQT9sw0Gp858gIh8lgTiUO90yIw)|

