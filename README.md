# miRNA–mRNA Networks in Primate Brain Development

This project examines gene expression and predicted microRNA (miRNA) regulation in the prefrontal cortex of humans, chimpanzees, and rhesus macaques.

**For full methods, figures, and results, see the [project report (Weeks 1–5)](./miRNA_mRNA_Project_Week1to5.docx) in the repository's main directory.**

---

## 1. Project Overview and Goals

We study how messenger RNA (mRNA) and miRNA expression change with developmental age and between primate species. The main goals are to identify early human–chimpanzee expression differences, assess their functional and autism-gene associations, and compare life-history trajectories of predicted miRNA–mRNA networks.

## 2. Source Data

- **Original study:** Somel, M., et al. (2011). [*MicroRNA-Driven Developmental Remodeling in the Brain Distinguishes Humans from Other Primates*](https://doi.org/10.1371/journal.pbio.1001214). *PLOS Biology*, **9**(12), e1001214.
- **Expression:** [GSE22521 (mRNA)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE22521) and [GSE29356 (miRNA)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE29356), plus published sample metadata and platform annotations.
- **miRNA–target information:** miRBase 12 and TargetScan 8.0 for miRNA names and seed families; miRDB v6.0 and human RefSeq annotations for predicted target interactions.
- **Functional annotations:** SFARI autism-gene lists and Gene Ontology (GO).

## 3. Analyses and Results

### a) Exploratory Data Analysis: Regression and Age Distribution

We cleaned and annotated **11,667 mRNA genes and 198 miRNAs**, plotted age distributions, and compared regression models using species and age. Notebook `3` displays species-specific regression trajectories against life-history-aligned age.

### b) Autism Analysis

Age-adjusted differential expression identified early human–chimpanzee candidate genes. Fisher's exact tests assessed enrichment for SFARI autism-associated genes.

### c) Functional Analysis with Gene Ontology

**GOfuncR** was used to identify GO biological processes associated with human-higher or chimpanzee-higher gene expression, with multiple-testing correction and hierarchy refinement.

### d) Life-History Network Analysis

Using **miRDB v6.0** predictions (score ≥ 80), we constructed a predicted seed-family–target-gene network and studied network features over life-history-aligned age. The uploaded notebook `5` matches **24** miRNA/mRNA profiles (7 human, 10 chimpanzee, 7 macaque). It standardizes miRNA and mRNA expression across all 24 profiles, computes network summaries for all 24, and uses **17 human–chimpanzee profiles** for the interaction and permutation tests. Profiles are joined by species and chronological age; a definitive accession-level donor validation is not encoded in the join.

## 4. Main Results

- Species and age together explained a mean **38.24% of mRNA** and **49.66% of miRNA** expression variation (mean in-sample $R^2$ across features; these are not out-of-sample accuracies).
- **140 genes** passed the early human–chimpanzee differential-expression criteria (**62 human-higher, 78 chimpanzee-higher**); no significant SFARI autism-gene enrichment was detected in one-sided Fisher tests.
- GO ranked-gene analysis (10,000 random sets) retained **10 human-higher** and **30 chimpanzee-higher** refined biological-process terms (FWER ≤ 0.05), highlighting transcription/RNA processing and neuronal/synaptic functions, respectively.
- The fixed miRDB network contains **156 seed families**, **7,039 target genes**, and **24,111 unique seed–gene edges** (not direct measurements of repression).
- Two life-history network features showed more negative age trends in humans: **target in-strength CV** (human–chimp slope difference **−0.010028**, BH FDR **0.013197**) and **edge-score dispersion** (difference **−0.005435**, BH FDR **0.045591**). The tests used **5,000 permutations** and BH correction across six features.

## 5. Execution and File Organization

Run Jupyter from the **main repository directory**, so relative paths such as `data/...` and `results/...` resolve correctly.

- **`data/`** contains original, downloaded input files and annotations (including any saved raw SFARI gene-table snapshot).
- **`results/`** contains **all generated intermediate files and analysis outputs** (CSV tables, plots, and RDS files). Subsequent notebooks load intermediates from this folder.
- The notebooks and this README are in the repository's main directory.

Execute notebooks in numerical order: **`1a` → `1b` → `2` → `3` → `4a` → `4b` → `5` → `6` → `7` → `8`**. Some branches are independent, but this order ensures prerequisites are prepared first. Notably, `1b` reads the all-samples exports from `1a`, while `6` reads the selected early-development cohort from `1b`; `7` and `8` both read the differential-expression results from `6`. The Python notebooks use a Python kernel; notebook `8` uses an R kernel.

**Raw input filenames expected in `data/`:** `GSE22521_series_matrix.txt`, `GSE22521_family.soft.gz`, `GPL6244.txt.gz`, `GSE29356_processed_data_pfc.txt`, `GSE29356_family.soft`, `miRBase12_human.csv`, `miR_Family_Info.txt`, `miRDB_v6.0_prediction_result.txt.gz`, and `GCF_000001405.40_GRCh38.p14_feature_table.txt.gz`. Notebook `7` additionally uses `sfari_human_genes_2026Q2.csv` as a required local source snapshot. The uploaded notebook does **not** download this file automatically if it is missing. The two derived SFARI category lists are saved to `results/`.

The notebooks save randomly selected miRNA and mRNA regression figures (`3`, `results/pfc_mirna_random_regressions.png` and `results/pfc_mrna_random_regressions.png`), sample-level network features, a network trajectory figure and permutation-test results (`5`), differential-expression tables and a saved distribution plot (`6`), Fisher-test tables (`7`), and GO tables, an RDS result object and a summary figure (`8`) in `results/`. Figures and tables are also displayed in their notebook cells. In notebook `6`, the optional 12-gene profile figure is **displayed but not saved** because its `savefig` command is intentionally commented out.

Large source and annotation files missing from the repository must be obtained from the original providers. The listed GEO series-matrix and miRNA processed-data paths currently expect **uncompressed `.txt` files**; `.gz` files are used directly where the notebook specifies the `.gz` extension.

## 6. Dependencies

### Python notebooks

The following are the **third-party packages imported by each notebook**. Python standard-library modules (such as `os`, `pathlib`, `sys`, and `importlib`) do not require a separate installation. The notebooks require **Python and a Jupyter/IPython kernel**; the recorded differential-expression environment used Python 3.12.14 and `pylimma` 0.2.0.

| Notebook | Python package dependencies |
|---|---|
| `1a_PFC_mRNA_check_data.ipynb` | `pandas`, `numpy`, `matplotlib`, `GEOparse`, `scikit-learn` |
| `1b_PFC_mRNA_human_specific.ipynb` | `pandas`, `numpy`, `matplotlib` |
| `2_PFC_miRNA_check_data.ipynb` | `pandas`, `numpy`, `matplotlib`, `GEOparse`, `scikit-learn` |
| `3_PFC_miRNA_mRNA_regression_plots.ipynb` | `pandas`, `numpy`, `matplotlib`, `IPython` |
| `4a_miRNA_seed_matching.ipynb` | `pandas`, `IPython` |
| `4b_miRNA_targets_matching_miRDB.ipynb` | `pandas`, `numpy`, `matplotlib`, `scikit-learn` |
| `5_miRNA_mRNA_network_analysis_miRDB.ipynb` | `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `scipy` |
| `6_PFC_early_dev_human_chimp_diff.ipynb` | `pandas`, `numpy`, `matplotlib`, `pylimma`, `statsmodels`, `IPython` |
| `7_PFC_early_dev_autism_genes_enrichment_with_Fisher.ipynb` | `pandas`, `numpy`, `scipy`, `IPython` |
| `8_PFC_GeneOntology_analysis.ipynb` | **R**, not Python; see below |

Install the Python packages into the environment used by your Jupyter kernel, for example:

```bash
python -m pip install jupyterlab ipykernel IPython pandas numpy matplotlib GEOparse scikit-learn scipy statsmodels pylimma
```

The package **`scikit-learn`** is imported as `sklearn`, and **`GEOparse`** is imported as `GEOparse`. The table lists imported packages; a few imports may be unused in individual notebooks. Package versions other than those noted above were not pinned or independently tested here.

### R notebook: `8_PFC_GeneOntology_analysis.ipynb`

- **Required R packages:** `GOfuncR` and `Homo.sapiens` (Bioconductor).
- **R notebook kernel:** `IRkernel` (required to run an R notebook in Jupyter).
- **Automatically installed dependencies** of these packages include annotation libraries such as `GO.db`, `org.Hs.eg.db`, `AnnotationDbi`, and other Bioconductor packages. These are normally resolved by Bioconductor; they do not need to be installed individually if dependency installation succeeds.
- The uploaded notebook records a successful run with **R 4.5.3**, **GOfuncR 1.30.0**, **Homo.sapiens 1.3.1**, and **IRkernel 1.3.2**. The notebook calls `sessionInfo()` to record R package versions.

For example, from an R session:

```r
install.packages(c("BiocManager", "IRkernel"))
BiocManager::install(c("GOfuncR", "Homo.sapiens"))
IRkernel::installspec(user = TRUE)
```

Then choose an **R kernel** when opening notebook `8`. Installation requires an R/Bioconductor environment compatible with the package versions. No `rpy2` bridge is required by this notebook.
