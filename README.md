# Activity Epigenomics - ATAC-seq and Distal Regulatory analysis

## Description

In this repository I have stored the following analyses:

* **Task 4:** EN-TEx ATAC-seq downstream analyses
* **Task 5:** Distal regulatory activity analysis

---

# Task 4 – EN-TEx ATAC-seq downstream analyses

## Main objectives

In this activity we have characterized chromatin accessibility in **sigmoid colon** and **stomach** tissues from the same EN-TEx donor by:

1. Retrieving **ATAC-seq pseudoreplicated peaks**
2. Identifying peaks overlapping **promoter regions**
3. Identifying peaks located **outside gene coordinates**

---

## 1. Data retrieval

We retrieved ATAC-seq metadata from ENCODE and perfomed a filtering according to:

* **Assay:** ATAC-seq
* **Format:** bigBed narrowPeak
* **Output type:** pseudoreplicated peaks
* **Assembly:** GRCh38
* **Tissues:** sigmoid colon and stomach

The selected files were:

| Tissue        | ENCODE accession |
| ------------- | ---------------- |
| sigmoid_colon | ENCFF287UHP      |
| stomach       | ENCFF762IFP      |

We performed validation of donwloaded files using **md5sum** and we did the conversion from **bigBed** to **BED** format.

### Total ATAC-seq peaks

After this procedure, we were able to identify the following peaks:

| Tissue        | Number of peaks |
| ------------- | --------------- |
| sigmoid_colon | 110,999         |
| stomach       | 103,609         |

---

## 2.Peak overlap analyses

Using protein-coding gene coordinates and promoter annotations, we performed genomic intersections with **BEDTools**.

### Peaks intersecting promoter regions

We obtained the promoter coordinates from:

`gencode.v24.protein.coding.non.redundant.TSS.bed`

And the intersecting peak promoters where:

| Tissue        | Peaks intersecting promoters |
| ------------- | ---------------------------- |
| sigmoid_colon | 47,871                       |
| stomach       | 44,749                       |

### Peaks outside gene coordinates

In this step whole gene body coordinates were used:

`gencode.v24.protein.coding.gene.body.bed`

The result of the number of peaks were:

| Tissue        | Peaks outside gene coordinates |
| ------------- | ------------------------------ |
| sigmoid_colon | 37,035                         |
| stomach       | 34,537                         |

### Explanation

We can see that a significant amount of accessible chromatin regions show ovverlapment with promoter regions. this is consistent with the fact that are trasncriptionally active locis. Additionally, we observed that many peaks were detected outside annotated gene bodies. This suggests the presence of distal regulatory elements.

---

# Task 5 – Distal regulatory activity analysis

## Description

In this activity I have identified **candidate distal regulatory elements** and I have assigned them to the closest protein-coding genes.

First, to identify candidate distal regulatory we looked at:

* ATAC-seq peaks **outside gene coordinates**
* That show overlapping both:

  * **H3K27ac** (active enhancer mark)
  * **H3K4me1** (enhancer-associated mark)

---

## ChIP-seq datasets used

These were the datasets used:

| Tissue        | Histone mark | ENCODE accession |
| ------------- | ------------ | ---------------- |
| sigmoid_colon | H3K27ac      | ENCFF872UHN      |
| sigmoid_colon | H3K4me1      | ENCFF724ZOF      |
| stomach       | H3K27ac      | ENCFF977LBD      |
| stomach       | H3K4me1      | ENCFF844XRN      |

---

## Candidate distal regulatory elements

In this actiivyt, only chromosome 1 was considered for downstream analyses. Showing the following candidate regulatory elements:

| Tissue        | Candidate distal regulatory elements |
| ------------- | ------------------------------------ |
| sigmoid_colon | 14,215                               |
| stomach       | 8,022                                |

---

## Nearest gene assignment

For this section we used a custom Python script (`get.distance.py`) to identify the closest protein-coding gene based on genomic distance to the gene start.

### Distance to nearest gene

We calculated the meand an median of the distances:

| Tissue        | Mean distance (bp) | Median distance (bp) |
| ------------- | ------------------ | -------------------- |
| sigmoid_colon | 72,978             | 35,802               |
| stomach       | 45,227             | 27,735               |

### Explanation

We observed that for both tissues, the **mean distance was higher than the median distance**. This suggests that most of the regulatory elements are located relatively close to genes, while a smaller subset lies much farther away.

These findings are consistent with what we shoudl expect for enhancer-mediated regulation, as we know that enhancers can act over distances ranging from several kilobases to hundreds of kilobases.

---

## Software and tools used

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
