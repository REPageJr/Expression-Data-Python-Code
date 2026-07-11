[README.md](https://github.com/user-attachments/files/29911492/README.md)
# System-level reorganization of transcriptomic networks underlies the nurse-to-forager transition in honey bees

Code and data accompanying the paper by **Robert E. Page, Jr.** and **Eric Bonabeau**
(School of Complex Adaptive Systems, Arizona State University, Tempe, AZ).

## Overview

This repository contains the analysis code and source data for a study of the
nurse-to-forager behavioral transition in worker honey bees (*Apis mellifera*),
analyzed from a network perspective. Rather than treating behavioral maturation
as incremental changes in individual genes, the paper treats the transcriptome
as a **signed gene–gene correlation system** and characterizes how its global
organization reorganizes across development.

Fat-body expression was measured for **90 target genes plus vitellogenin (VG)
protein titers** in **80 workers** sampled at **5 ages (days 1, 3, 6, 10, 15)**
(16 bees per age). From these data the code builds signed gene–gene correlation
networks and quantifies their structure at three levels:

- **Macrostate** — signed modularity, signed algebraic connectivity (λ₂), and
  spectral gap of the signed Laplacian, each compared against permutation nulls
  and bootstrap confidence intervals.
- **Mesostate** — force-directed network layouts, consensus clustering into
  modules, and cluster-to-cluster tracking across ages (Sankey diagram).
- **Microstate** — gene-level module membership (a pro-VG "M1" module and an
  antagonistic anti-VG "M2" module), PCA/MDS trajectories, and a VG-knockdown
  (TYR1) validation experiment analyzed with LDA.

## Manuscript files (Word documents)

| File | Contents |
|------|----------|
| `Revised Version 6_27_26.docx` | Main manuscript (title, abstract, significance, introduction, results, methods). |
| `Supporting Information 6_27_26.docx` | Supporting information and supplementary figure captions. |

## Data files

| File | Description |
|------|-------------|
| `Normalized Data.xlsx` | Primary dataset. 80 rows (bees) × 92 columns: `Age (Days)`, 90 normalized gene-expression columns, and `Vg protein normalized` (VG titer). Used by nearly every analysis. |
| `Knockdown Data.xlsx` | TYR1-knockdown validation experiment. 79 samples × 29 columns of RQ / log-RQ values for a panel of genes, plus `treatment` (Control / TYR-), `Round`, and `Time`. Used for the LDA knockdown analysis. |
| `consensus_clusters_day1_K4.csv` | Consensus-clustering module memberships, Day 1 (K=4). |
| `consensus_clusters_day3_K3.csv` | Consensus-clustering module memberships, Day 3 (K=3). |
| `consensus_clusters_day6_K3.csv` | Consensus-clustering module memberships, Day 6 (K=3). |
| `consensus_clusters_day10_K4.csv` | Consensus-clustering module memberships, Day 10 (K=4). |
| `consensus_clusters_day15_K4.csv` | Consensus-clustering module memberships, Day 15 (K=4). |

Each consensus-cluster CSV has columns:
`feature, cluster, stability, Vg_rho, Vg_p, module, module_size, module_mean_Vg_rho, module_stability`,
where `feature` is the gene (or `Vg protein normalized`), `module` is the
relabeled module (`M1` = pro-VG, `M2` = anti-VG), and `Vg_rho`/`Vg_p` are the
Spearman correlation of that gene with VG on that day.

## Code

`Code for Paper.ipynb` is a Jupyter notebook. Each code cell is self-contained,
begins with a comment naming the figure it produces, and reads directly from the
data files above. Cell-to-figure mapping:

| Notebook cell | Figure | Analysis |
|---------------|--------|----------|
| 1  | Fig. 1A | Signed modularity vs. permutation null with bootstrap CIs. |
| 2  | Fig. 1B, 1C | Signed algebraic connectivity (λ₂) and spectral gap. |
| 3  | Fig. S1 | Edge-level statistics; day 6 → day 10 transition test. |
| 4  | Fig. 2 | Force-directed (spring) layouts of the correlation networks. |
| 5  | Fig. S2 | Vitellogenin dynamics across age (Kruskal–Wallis, Levene, Dunn). |
| 6  | Fig. 3 | Pairwise consensus-partition agreement (AMI / NMI / ARI). |
| 7  | Fig. 4 | Sankey diagram of module flow across days (Hungarian re-alignment). |
| 8  | Fig. S3A | Gene–VG correlation structure. |
| 9  | Fig. S3B | Day-15 M1/M2 module correlation heatmap. |
| 10 | Fig. 5 | Module-level (M1 vs. M2) network analysis. |
| 11 | Fig. S5 | Correlation-structure supplement. |
| 12 | Fig. S4 | Per-day pairwise-correlation distributions. |
| 13 | Fig. 6 | PC1 top-20 gene loadings by age. |
| 14 | Fig. 7–8 | PCA / MDS developmental trajectories. |
| 15 | Fig. S6 (knockdown) | TYR1-knockdown LDA projection of control vs. knockdown vs. age groups. |
| 16 | Fig. S6 stats | Stratified bootstrap CIs for the LDA-projected scores. |
| 17 | Supplementary | 90-panel grid of per-gene expression across age. |

> Note: figure numbers follow the comment at the top of each cell and the
> manuscript; the notebook comments occasionally use working labels (e.g.
> "Figure 15") that correspond to the knockdown supplement.

## Requirements

The notebook was developed and run on **Python 3.12** (Jupyter kernel
`general312`). Required packages:

- `numpy`, `pandas`, `scipy`, `matplotlib`, `seaborn`
- `scikit-learn`
- `statsmodels`
- `networkx`
- `plotly` (Sankey diagram, cell producing Fig. 4)
- `scikit-posthocs` (Dunn's test, Fig. S2)
- `openpyxl` (reading the `.xlsx` data files)

Pinned versions are listed in [`requirements.txt`](requirements.txt). Install
everything with:

```bash
pip install -r requirements.txt
```

Or install the latest versions directly:

```bash
pip install numpy pandas scipy matplotlib seaborn scikit-learn statsmodels \
    networkx plotly scikit-posthocs openpyxl
```

## Running

1. Keep the notebook and all data files (`.xlsx`, `.csv`) in the same directory —
   the code loads them by relative filename.
2. Open the notebook and run cells top to bottom, or run any single figure cell
   independently (most cells re-import their dependencies and reload the data).
3. Each cell writes its output figure(s) (`.png` / `.html`) and, where relevant,
   a summary `.csv` into the working directory.

A fixed `RANDOM_STATE = 42` is used for permutation, bootstrap, and clustering
routines so results are reproducible.

## Citation

Page, R. E., Jr. & Bonabeau, E. *System-level reorganization of transcriptomic
networks underlies the nurse-to-forager transition in honey bees.*
