# Integrated Differential Gene Expression, Functional Enrichment and Machine Learning Analysis of PCOS Using Cumulus Cell RNA-Seq Data

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/yyymaaa/PCOS_ML_analysis/blob/main/PCOS_Project.ipynb)

## Research question
Which genes and biological pathways are differentially expressed in cumulus cells from women with Polycystic Ovary Syndrome (PCOS) compared with non-PCOS controls, and can these gene expression differences be used to accurately classify samples into the two groups?

## Objectives
1. Obtain, preprocess and quality-check public RNA-seq data from PCOS and control cumulus cells.
2. Identify differentially expressed genes using statistical testing with false discovery rate (FDR) correction.
3. Interpret the biological role of these genes through pathway enrichment and a protein-protein interaction network.
4. Build and evaluate machine learning models that classify PCOS versus control samples.

## Dataset
| Item | Detail |
|---|---|
| Source | NCBI Gene Expression Omnibus, [GSE277906](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE277906) |
| Tissue | Human cumulus cells |
| Platform | RNA-seq, Illumina NovaSeq 6000 |
| Samples | 40 (23 PCOS, 17 control) |
| Files | Raw counts and FPKM (`GSE277906_counts_anno.txt.gz`, `GSE277906_FPKM_anno.txt.gz`) |

The notebook downloads the data automatically from the GEO FTP site, so no manual download is needed.

## Workflow
1. **Data acquisition:** download and load counts and FPKM matrices
2. **Quality control:** check for missing, negative, non-integer and duplicate values; review library sizes; keep genes with at least 10 reads in at least 2 samples (19,216 genes kept)
3. **Exploratory analysis:** box plots, sample correlation heatmap, PCA
4. **Differential expression:** PyDESeq2, PCOS vs Control, Benjamini-Hochberg correction
5. **Validation:** sample alignment checks and outlier review
6. **Functional enrichment:** GO Biological Process and KEGG via Enrichr (gseapy)
7. **Network analysis:** STRING-DB interactions visualised in Cytoscape
8. **Machine learning:** Random Forest and Logistic Regression vs a baseline, with leakage-free 5-fold cross-validation repeated 10 times

## Key results
- **15 genes** significantly differentially expressed (padj < 0.05), all **lower in PCOS**, including S100A9, PECAM1, ICAM1, RUNX3, C1QB and CFD.
- Enriched pathways: endothelial development, leukocyte adhesion, extracellular-matrix organisation and complement regulation.
- The network formed two clusters: **immune signalling and cell adhesion** (ICAM1, PECAM1, S100A9, CSF3, TNFAIP2) and the **complement cascade** (CFD, C2, C1QB).
- **Machine learning:** with genes selected inside each training fold, no model beat the baseline.

| Model | Accuracy (mean ± SD) | ROC AUC (mean ± SD) |
|---|---|---|
| Baseline (always predicts PCOS) | 0.57 ± 0.06 | 0.50 ± 0.00 |
| Logistic Regression | 0.57 ± 0.18 | 0.58 ± 0.23 |
| Random Forest | 0.56 ± 0.17 | 0.56 ± 0.23 |

**Conclusion:** PCOS cumulus cells show a real, group-level drop in immune, complement and adhesion genes, but with 40 samples these differences do not reliably classify individual women.

## How to run
1. Click **Open in Colab** above.
2. Go to **Runtime → Run all**.
3. Required packages (`pydeseq2`, `gseapy`) are installed inside the notebook.

## Tools
Python, pandas, NumPy, matplotlib, seaborn, scikit-learn, PyDESeq2, gseapy (Enrichr), STRING-DB, Cytoscape, Google Colab.

## Team (Group Three)
| Member | Role |
|---|---|---|
| 166993 | Nancy Njoroge | Data acquisition |
| 168000 | Macklee Gitonga | Data preprocessing and quality control |
| 169393 | Bilgis Nzembi | Exploratory data analysis |
| 169786 | Nicole Agufana | Core analysis (differential expression) |
| 168222 | Phoebe Mokoit | Biological interpretation |
| 162396 | Amy Ojuka | Machine learning classification and conclusions |

## Limitations
Small sample size (40), bulk RNA-seq (averages across cells), all participants undergoing IVF, no clinical covariates, and several significant genes are immune/vascular cell markers.

## References
- GEO dataset GSE277906, NCBI Gene Expression Omnibus
- Sikiru et al. (2025), *Middle East Fertility Society Journal*, [doi:10.1186/s43043-025-00241-w](https://doi.org/10.1186/s43043-025-00241-w)
- [PyDESeq2 documentation](https://pydeseq2.readthedocs.io/en/latest/)
