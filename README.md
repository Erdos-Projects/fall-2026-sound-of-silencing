# miRNA–mRNA Networks in Primate Brain Development

This project examines gene expression and predicted microRNA (miRNA) regulation in the prefrontal cortex of humans, chimpanzees, and rhesus macaques.

**For full methods, figures, and results, see the [project report (Weeks 1-5)](./miRNA_mRNA_Project_Week1to5.pdf) in the repository's main directory.**

---

## 1. Project Overview and Goals

We study how messenger RNA (mRNA) and miRNA expression change with developmental age and between primate species. The main goals are to identify early human–chimpanzee expression differences, assess their functional and autism-gene associations, and compare life-history trajectories of predicted miRNA–mRNA networks.

The current workflow incorporates the Week 5 review revisions: cross-species probe-mask verification, batch-adjusted differential expression, protein-coding gene annotation, revised SFARI Fisher tests, network-specificity tests, and correlation-aware Gene Ontology sensitivity analyses.

## 2. Source Data

- **Original study:** Somel, M., et al. (2011). [*MicroRNA-Driven Developmental Remodeling in the Brain Distinguishes Humans from Other Primates*](https://doi.org/10.1371/journal.pbio.1001214). *PLOS Biology*, **9**(12), e1001214.
- **Expression:** [GSE22521 (mRNA)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE22521) and [GSE29356 (miRNA)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE29356), plus published sample metadata and platform annotations.
- **miRNA–target information:** miRBase 12 and TargetScan 8.0 for miRNA names and seed families; miRDB v6.0 and human RefSeq annotations for predicted target interactions.
- **Functional annotations:** SFARI autism-gene lists and Gene Ontology (GO).
- **Literature context for the SFARI result:** Starr, A. L. & Fraser, H. B. (2025). *A general principle of neuronal evolution reveals a human-accelerated neuron type potentially underlying the high prevalence of autism in humans*. *Molecular Biology and Evolution*, 42(9), msaf189. https://doi.org/10.1093/molbev/msaf189

## 3. Analyses and Results

### a) Exploratory Data Analysis: Regression and Age Distribution

We cleaned and annotated **11,667 mRNA genes and 198 miRNAs**, plotted age distributions, and compared regression models using species and age. Notebook `3` displays species-specific regression trajectories against life-history-aligned age.

The updated `1a` notebook also verifies that the processed GSE22521 matrix already incorporates the cross-species probe mask used by Somel et al.: retained probes matched the human, chimpanzee, and rhesus macaque genomes perfectly and uniquely before downstream normalization and summarization.

### b) Early Human–Chimpanzee Differential Expression and Autism Analysis

Notebook `6a` performs the early-development differential-expression analysis and adds GEO batch as a sensitivity covariate. The batch-adjusted model selects **251 genes**: **131 human-higher** and **120 chimpanzee-higher**. The original and batch-adjusted species coefficients remain strongly correlated, indicating that explicit batch adjustment does not remove the early human–chimpanzee signal.

Notebook `6b` annotates the 251 selected genes by current human gene type. Of these, **225 are protein-coding**, 10 are pseudogenes, 9 are lncRNA/noncoding genes, and 7 are unmapped. The protein-coding lists are used by the revised GO analysis.

Notebook `7` reruns the SFARI analysis using the **251 batch-adjusted genes**. Two-sided Fisher exact tests show **underrepresentation of SFARI genes** among the selected genes, and both comparisons remain significant after correction across the two SFARI tests:

- **SFARI Category 1:** 0 of 251 selected genes; two-sided p = 0.01584; Bonferroni-adjusted p = 0.03168.
- **SFARI Categories 1 + 2:** 6 of 251 selected genes; odds ratio = 0.3606; two-sided p = 0.00782; Bonferroni-adjusted p = 0.01563.

This depletion is broadly consistent with recent primate-neurodevelopment work reporting non-random evolutionary behavior of high-confidence autism genes, although the published study and the present bulk-tissue Fisher analysis are not identical tests.

### c) Functional Analysis with Gene Ontology

The current GO workflow uses the **11,052 protein-coding genes** from notebook `6b`, the batch-adjusted differential-expression statistics from `6a`, and several complementary tests: ranked GOfuncR, sample-level Freedman–Lane permutation tests, and CAMERA analyses that account for inter-gene correlation.

**Exploratory observation.** Nominal GO signals point in two directions: chimpanzee-higher expression of synaptic and neuronal-signalling genes, and human-higher expression of mitotic spindle-checkpoint, chromosome-segregation and DNA-repair genes. Neither theme passes BH FDR ≤ 0.10 in any correlation-aware test. Ranked GOfuncR identifies many terms under its gene-level randomization null, but the correlation-aware analyses are much more conservative. We therefore treat the GO results as exploratory and requiring verification in an independent study.

The current GO notebook uses **10,000 random sets / sample-level permutations** where applicable.

### d) Life-History Network Analysis

Using **miRDB v6.0** predictions (score ≥ 80), we constructed a predicted seed-family–target-gene network and studied network features over life-history-aligned age. Notebook `5` matches **24** miRNA/mRNA profiles (7 human, 10 chimpanzee, 7 macaque), standardizes miRNA and mRNA expression across all 24 profiles, and uses the **17 human–chimpanzee profiles** for interaction and permutation tests.

The fixed network contains **156 seed families**, **7,039 target genes**, and **24,111 unique seed–gene edges**.

The original network analysis identified two significant developmental parameters: target in-strength CV and edge-score dispersion. However, these effects **did not withstand target-gene label randomization**, indicating that they are not specific to the biological miRNA–target assignments. Further analysis showed that the network summaries track simpler expression heterogeneity. In particular, mRNA expression SD decreases more strongly with age in humans than in chimpanzees (Freedman–Lane p = 0.0114; BH FDR = 0.0456). The current interpretation is therefore that the apparent network signal may reflect a **developmental decrease in expression heterogeneity in humans**, rather than network-specific miRNA repression.

Profiles are joined by species and chronological age; definitive accession-level donor validation is not encoded in the join.

## 4. Main Results

- Species and age together explained a mean **38.24% of mRNA** and **49.66% of miRNA** expression variation in the exploratory regression analysis (mean in-sample \(R^2\) across features; these are not out-of-sample accuracies).
- The batch-adjusted early human–chimpanzee analysis selected **251 genes** (**131 human-higher, 120 chimpanzee-higher**).
- Of the 251 selected genes, **225 (89.6%) are protein-coding**.
- SFARI autism-associated genes are **underrepresented** among the 251 selected genes, with both two-sided Fisher tests remaining significant after correction across the two SFARI comparisons.
- **GO interpretation is exploratory:** nominal chimpanzee-higher synaptic/neuronal-signalling and human-higher mitotic/checkpoint/chromosome/DNA-repair themes do not pass BH FDR ≤ 0.10 in the correlation-aware analyses.
- The miRDB network contains **156 seed families**, **7,039 target genes**, and **24,111 unique seed–gene edges**.
- The two originally significant network parameters do **not** survive network target-label randomization. The remaining developmental signal is better described as an **expression-heterogeneity effect**, with mRNA expression heterogeneity decreasing more strongly with age in humans.

## 5. Current Notebook Workflow and File Organization

Run Jupyter from the **main repository directory**, so relative paths such as `data/...` and `results/...` resolve correctly.

- **`data/`** contains original downloaded input files and annotations, including the saved SFARI source snapshot.
- **`results/`** contains generated intermediate files and analysis outputs (CSV tables, plots, and RDS files). Later notebooks load intermediates from this folder.
- **`old/`** contains the pre-review notebook versions. These are retained for provenance and to match the historical Week 1–5 sections of the report, but they are not part of the current analysis workflow.
- The current notebooks and this README are in the repository's main directory.

Execute the **current** notebooks in this order:

1. `1a_PFC_mRNA_check_data.ipynb`
2. `1b_PFC_mRNA_human_specific.ipynb`
3. `2_PFC_miRNA_check_data.ipynb`
4. `3_PFC_miRNA_mRNA_regression_plots.ipynb`
5. `4a_miRNA_seed_matching.ipynb`
6. `4b_miRNA_targets_matching_miRDB.ipynb`
7. `5_miRNA_mRNA_network_analysis_miRDB_network.ipynb`
8. `6a_PFC_early_dev_human_chimp_diff.ipynb`
9. `6b_PFC_human_chimp_early_diff_proteins.ipynb`
10. `7_PFC_early_dev_autism_genes_enrichment_with_Fisher.ipynb`
11. `8_PFC_GeneOntology_protein_coding.ipynb`

Key dependencies:

- `1b` reads the all-samples exports from `1a`.
- `5` reads processed mRNA/miRNA data plus the miRNA seed annotation and miRDB network from `4a`/`4b`.
- `6a` reads the selected early-development cohort from `1b` and produces the batch-adjusted 251-gene analysis.
- `6b` reads the batch-adjusted output from `6a` and creates the protein-coding universe and selected-gene lists.
- `7` reads the batch-adjusted 251-gene result from `6a`.
- `8` reads the batch-adjusted result from `6a` and the protein-coding gene lists from `6b`.

Archived pre-review notebooks include:

- `old/1a_PFC_mRNA_check_data_old.ipynb`
- `old/5_miRNA_mRNA_network_analysis_miRDB_old.ipynb`
- `old/6_PFC_early_dev_human_chimp_diff_old.ipynb`
- `old/7_PFC_early_dev_autism_genes_enrichment_with_Fisher_old.ipynb`
- `old/8_PFC_GeneOntology_analysis_old.ipynb`

### Raw input filenames expected in `data/`

`GSE22521_series_matrix.txt`, `GSE22521_family.soft.gz`, `GPL6244.txt.gz`, `GSE29356_processed_data_pfc.txt`, `GSE29356_family.soft`, `miRBase12_human.csv`, `miR_Family_Info.txt`, `miRDB_v6.0_prediction_result.txt.gz`, and `GCF_000001405.40_GRCh38.p14_feature_table.txt.gz`.

Notebook `7` additionally uses `sfari_human_genes_2026Q2.csv` as the required local SFARI source snapshot.

### Important generated outputs

- `1a`: processed mRNA expression/metadata, including the all-sample exports used by `1b`.
- `3`: randomly selected mRNA and miRNA regression figures.
- `5`: network sample features, original six-feature permutation tests, network-specificity randomization results, expression-heterogeneity results, and the mRNA heterogeneity trajectory figure.
- `6a`: original and batch-adjusted differential-expression tables plus batch-adjusted distribution/volcano and selected-gene profile plots.
- `6b`: selected-gene biotype tables and protein-coding gene lists.
- `7`: SFARI contingency tables and the two-sided Fisher / multiple-testing-correction table.
- `8`: protein-coding GOfuncR, Freedman–Lane, CAMERA, and sensitivity-analysis output tables.

Large source and annotation files missing from the repository must be obtained from the original providers. The listed GEO series-matrix and miRNA processed-data paths currently expect uncompressed `.txt` files where specified; `.gz` files are read directly where the notebook filename includes `.gz`.

## 6. Dependencies

### Python notebooks

The notebooks require **Python and a Jupyter/IPython kernel**. The recorded differential-expression environment used Python 3.12.14 and `pylimma` 0.2.0.

| Notebook | Main third-party dependencies |
|---|---|
| `1a_PFC_mRNA_check_data.ipynb` | `pandas`, `numpy`, `matplotlib`, `GEOparse`, `scikit-learn` |
| `1b_PFC_mRNA_human_specific.ipynb` | `pandas`, `numpy`, `matplotlib` |
| `2_PFC_miRNA_check_data.ipynb` | `pandas`, `numpy`, `matplotlib`, `GEOparse`, `scikit-learn` |
| `3_PFC_miRNA_mRNA_regression_plots.ipynb` | `pandas`, `numpy`, `matplotlib`, `IPython` |
| `4a_miRNA_seed_matching.ipynb` | `pandas`, `IPython` |
| `4b_miRNA_targets_matching_miRDB.ipynb` | `pandas`, `numpy`, `matplotlib`, `scikit-learn` |
| `5_miRNA_mRNA_network_analysis_miRDB_network.ipynb` | `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `scipy` |
| `6a_PFC_early_dev_human_chimp_diff.ipynb` | `pandas`, `numpy`, `matplotlib`, `pylimma`, `statsmodels`, `IPython` |
| `6b_PFC_human_chimp_early_diff_proteins.ipynb` | `pandas`, `numpy`, `mygene` |
| `7_PFC_early_dev_autism_genes_enrichment_with_Fisher.ipynb` | `pandas`, `numpy`, `scipy`, `IPython` |
| `8_PFC_GeneOntology_protein_coding.ipynb` | **R**, not Python; see below |

Install the Python packages into the environment used by your Jupyter kernel, for example:

```bash
python -m pip install jupyterlab ipykernel IPython pandas numpy matplotlib GEOparse scikit-learn scipy statsmodels pylimma mygene
```

`6b` queries MyGene.info through the `mygene` package, so internet access is required when that annotation step is rerun.

### R notebook: `8_PFC_GeneOntology_protein_coding.ipynb`

The revised GO notebook uses an R kernel.

Required packages include:

- `GOfuncR`
- `Homo.sapiens`
- `limma`
- `AnnotationDbi`
- `org.Hs.eg.db`
- `GO.db`
- `Matrix`
- `IRkernel`

For example, from an R session:

```r
install.packages(c("BiocManager", "IRkernel"))
BiocManager::install(c(
    "GOfuncR",
    "Homo.sapiens",
    "limma",
    "AnnotationDbi",
    "org.Hs.eg.db",
    "GO.db"
))
IRkernel::installspec(user = TRUE)
```

Then choose an **R kernel** when opening notebook `8`. No `rpy2` bridge is required.
