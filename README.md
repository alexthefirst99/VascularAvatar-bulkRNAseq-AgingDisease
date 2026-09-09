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
    └── Fig4A_VSMC_UMAP.ipynb                   — Figure 4A
```

---

## Notebooks

### `notebooks/Fig1E_HAEC_inflammation_heatmap.ipynb` — Figure 1E
Row-normalized, hierarchically-clustered heatmap of inflammation-associated marker genes across primary HAEC at day 0, day 40 (control medium), and day 40 + VEGF.
- **Input**: `Fig1E_HAEC_FPKM.csv` (3 samples: `D0`, `D40_Ctrl`, `D40_VEGF`)
- **Pipeline**: log10(FPKM+1) on the curated marker panel → row min-max normalization → hierarchical clustering of genes (samples kept in chronological order)

### `notebooks/Fig2D_iPSC_EC_UMAP.ipynb` — Figure 2D
PCA + UMAP of iPSC-derived EC across the 9 differentiation/maintenance conditions (VSL and its variants) at day 0 and day 40.
- **Input**: `Fig2D_iPSC_EC_FPKM.csv` (27 samples, 9 conditions × 3 replicates)
- **Pipeline**: log2(FPKM+1) → filter genes detected in ≥3 samples → top 1000 most-variable genes → PCA (centered, not scaled) → UMAP on the PCA coordinates

### `notebooks/Fig4A_VSMC_UMAP.ipynb` — Figure 4A
PCA + UMAP of iPSC-derived VSMC across the VSL protocol and its variants (FGF-depleted, 8Br-cAMP, PDGF inhibitor, mVSL) vs. day 0 and day 40 controls.
- **Input**: `Fig4A_VSMC_FPKM.csv` (11 samples, 7 conditions)
- **Pipeline**: same as Figure 2D above

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
packages: pandas, numpy, scikit-learn, umap-learn, matplotlib, seaborn, nbformat
```

---

## Citation

Qin W, et al. "A Long-lived Avatar for Modeling Age-Related Vascular Disease." *Manuscript in preparation.*

---

## Contact

Tu N. Tran — ttran7@houstonmethodist.org | Wang Lab, Houston Methodist
