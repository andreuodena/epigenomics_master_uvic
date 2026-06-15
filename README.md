# Epigenomics UVic – ATAC-seq and Distal Regulatory Activity Analysis

## Overview

This repository contains the analyses performed for the **Epigenomics course (UVic-UCC Master in Omics Data Analysis)**, focused on:

* **Task 4:** EN-TEx ATAC-seq downstream analyses
* **Task 5:** Distal regulatory activity analysis

The analyses were performed using **Docker**, **BEDTools**, **ENCODE EN-TEx datasets**, and custom Python scripts.

---

# Task 4 – EN-TEx ATAC-seq downstream analyses

## Objective

The goal of this task was to characterize chromatin accessibility in **sigmoid colon** and **stomach** tissues from the same EN-TEx donor by:

1. Retrieving **ATAC-seq pseudoreplicated peaks**
2. Identifying peaks overlapping **promoter regions**
3. Identifying peaks located **outside gene coordinates**

---

## Data retrieval

ATAC-seq metadata were retrieved from ENCODE and filtered according to:

* **Assay:** ATAC-seq
* **Format:** bigBed narrowPeak
* **Output type:** pseudoreplicated peaks
* **Assembly:** GRCh38
* **Tissues:** sigmoid colon and stomach

Selected files:

| Tissue        | ENCODE accession |
| ------------- | ---------------- |
| sigmoid_colon | ENCFF287UHP      |
| stomach       | ENCFF762IFP      |

Downloaded files were validated using **md5sum** and converted from **bigBed** to **BED** format using `bigBedToBed`.

### Total ATAC-seq peaks

| Tissue        | Number of peaks |
| ------------- | --------------- |
| sigmoid_colon | 110,999         |
| stomach       | 103,609         |

---

## Peak overlap analyses

Protein-coding gene coordinates and promoter annotations were used to perform genomic intersections with **BEDTools**.

### Peaks intersecting promoter regions

Promoter coordinates were obtained from:

`gencode.v24.protein.coding.non.redundant.TSS.bed`

| Tissue        | Peaks intersecting promoters |
| ------------- | ---------------------------- |
| sigmoid_colon | 47,871                       |
| stomach       | 44,749                       |

### Peaks outside gene coordinates

Whole gene body coordinates were used:

`gencode.v24.protein.coding.gene.body.bed`

| Tissue        | Peaks outside gene coordinates |
| ------------- | ------------------------------ |
| sigmoid_colon | 37,035                         |
| stomach       | 34,537                         |

### Interpretation

A substantial proportion of accessible chromatin regions overlapped promoter regions, consistent with transcriptionally active genomic loci. Additionally, many peaks were detected outside annotated gene bodies, suggesting the presence of distal regulatory elements such as enhancers.

---

# Task 5 – Distal regulatory activity analysis

## Objective

The objective of this task was to identify **candidate distal regulatory elements** and assign them to the nearest protein-coding genes.

Candidate distal regulatory regions were defined as:

* ATAC-seq peaks **outside gene coordinates**
* Overlapping both:

  * **H3K27ac** (active enhancer mark)
  * **H3K4me1** (enhancer-associated mark)

---

## ChIP-seq datasets used

| Tissue        | Histone mark | ENCODE accession |
| ------------- | ------------ | ---------------- |
| sigmoid_colon | H3K27ac      | ENCFF872UHN      |
| sigmoid_colon | H3K4me1      | ENCFF724ZOF      |
| stomach       | H3K27ac      | ENCFF977LBD      |
| stomach       | H3K4me1      | ENCFF844XRN      |

---

## Candidate distal regulatory elements

Only chromosome 1 was considered for downstream analyses.

| Tissue        | Candidate distal regulatory elements |
| ------------- | ------------------------------------ |
| sigmoid_colon | 14,215                               |
| stomach       | 8,022                                |

---

## Nearest gene assignment

A custom Python script (`get.distance.py`) was implemented to identify the nearest protein-coding gene based on genomic distance to the gene start coordinate.

### Distance to nearest gene

| Tissue        | Mean distance (bp) | Median distance (bp) |
| ------------- | ------------------ | -------------------- |
| sigmoid_colon | 72,978             | 35,802               |
| stomach       | 45,227             | 27,735               |

### Interpretation

In both tissues, the **mean distance was higher than the median distance**, suggesting a **right-skewed distribution** in which most regulatory elements are located relatively close to genes, while a smaller subset lies much farther away.

These observations are biologically consistent with enhancer-mediated regulation, as enhancers can act over distances ranging from several kilobases to hundreds of kilobases.

---

## Software and tools

* Docker image: `dgarrimar/epigenomics_course`
* BEDTools
* bigBedToBed
* awk
* Python
* ENCODE EN-TEx datasets

---

## Repository structure

```text
ATAC-seq/
├── analyses/
├── annotation/
└── data/

ChIP-seq/
├── annotation/
├── analyses/
└── data/

regulatory_elements/

bin/
└── get.distance.py
```
