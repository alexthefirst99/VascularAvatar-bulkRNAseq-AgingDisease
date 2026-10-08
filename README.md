# A Long-lived Avatar for Modeling Age-Related Vascular Disease

**Bulk RNA sequencing analysis | Wang Lab | Houston Methodist**

---

## Overview

This repository contains analysis scripts for the bulk RNA sequencing (RNA-seq) component of the Long-lived Vascular Avatar study. Primary human aortic endothelial cells (HAEC), human iPSC-derived endothelial cells (iPSC-EC), and human iPSC-derived vascular smooth muscle cells (iPSC-VSMC) were profiled to characterize the VSL (VEGF + SB431542) differentiation/maintenance protocol used to model vascular aging.

Each notebook takes a processed Cufflinks/Cuffdiff FPKM matrix as input and reproduces one manuscript figure.

---

## Repository Contents

```
├── README.md
└── notebooks/
    ├── Fig1E_HAEC_inflammation_heatmap.ipynb   — Figure 1E
    ├── Fig2D_iPSC_EC_UMAP.ipynb                — Figure 2D
    ├── Fig4A_VSMC_UMAP.ipynb                   — Figure 4A
    └── iPSC_EC_Day60_marker_violin.ipynb       — iPSC-EC day 60 marker violins
```

---

## Notebooks

### `notebooks/Fig1E_HAEC_inflammation_heatmap.ipynb` — Figure 1E
Row-normalized, hierarchically-clustered heatmap of inflammation-associated marker genes across primary HAEC at day 0, day 40 (control medium), and day 40 + VEGF.
- **Pipeline**: log10(FPKM+1) on the curated marker panel → row min-max normalization → hierarchical clustering of genes (samples kept in chronological order)

### `notebooks/Fig2D_iPSC_EC_UMAP.ipynb` — Figure 2D
PCA + UMAP of iPSC-derived EC across the 9 differentiation/maintenance conditions (VSL and its variants) at day 0 and day 40.
- **Pipeline**: log2(FPKM+1) → filter genes detected in ≥3 samples → top 1000 most-variable genes → PCA (centered, not scaled) → UMAP on the PCA coordinates

### `notebooks/Fig4A_VSMC_UMAP.ipynb` — Figure 4A
PCA + UMAP of iPSC-derived VSMC across the VSL protocol and its variants (FGF-depleted, 8Br-cAMP, PDGF inhibitor, mVSL) vs. day 0 and day 40 controls.
- **Pipeline**: same as Figure 2D above

### `notebooks/iPSC_EC_Day60_marker_violin.ipynb` — iPSC-EC day 60 marker violins
Violin plots of six endothelial and six fibroblast markers across the two day-60 iPSC-EC media conditions, with the three biological replicates overlaid as dots.
- **Pipeline**: concatenate batch 1/2 + batch 3 → filter genes detected in ≥3 samples → total-count normalization to 10,000 per sample → log1p → restrict to the highly-variable gene list → marker panel → pool replicates per condition
- **Statistics**: two-sided Welch t-test, n = 3 vs. n = 3. Only *CD34* reaches p < 0.05 (p = 0.036) and no gene survives multiple-testing correction across the twelve markers, so the brackets are descriptive. `SHOW_STATS = False` omits them.

Note this notebook uses log1p of total-count-normalized values, not log2/log10(FPKM+1) as above, so its y-axis is a log-normalized expression value. The batch 1/2 matrix is required even though only day-60 samples are plotted, because per-sample normalization totals are computed over the genes retained across all samples.

---

## Pipeline reference (Fig 2D, Fig 4A)

Standard bulk RNA-seq exploratory-analysis workflow, in Python (`pandas` / `numpy` / `scikit-learn` / `umap-learn`):

1. log2(FPKM + 1).
2. Keep genes detected (FPKM > 0) in at least 3 samples.
3. Top 1000 most-variable genes across samples.
4. PCA, mean-centered but not scaled to unit variance.
5. UMAP on the PCA coordinates (`umap-learn`), `n_neighbors=8` (Fig 2D) / `n_neighbors=3` (Fig 4A), `min_dist=0.5`, fixed `random_state`.

---

## Data Availability

Raw FASTQ files and processed FPKM matrices are available at NCBI GEO (accession: pending).

---

## Environment

```
python >= 3.9
packages: pandas, numpy, scipy, scikit-learn, umap-learn, matplotlib, seaborn, nbformat
marker violin notebook also requires: scanpy, anndata
```

---

## Citation

Qin W, et al. "A Long-lived Avatar for Modeling Age-Related Vascular Disease." *Manuscript in preparation.*

---

## Contact

Tu N. Tran — ttran7@houstonmethodist.org | Wang Lab, Houston Methodist
