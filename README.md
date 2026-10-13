
# Spatial Gene Expression Imputation in Colorectal Cancer

### Integration of Single-Cell RNA Sequencing and Xenium Spatial Transcriptomics

**Project:** Applied Computational Genomics — Fall 2026  
**Research Area:** Computational Genomics | Spatial Transcriptomics | Bioinformatics  
**Programming Language:** Python  
**Status:** Data preprocessing completed; gene harmonization and imputation in development

---

## 1. Project Overview

Spatial transcriptomics enables the characterization of gene expression within its original tissue context, providing information about cellular organization and spatial relationships that is not preserved by conventional single-cell RNA sequencing (scRNA-seq).

However, targeted spatial transcriptomics technologies, including earlier versions of the 10x Genomics Xenium platform, measure a predefined subset of genes rather than the complete transcriptome. This limits the investigation of genes and biological pathways that were not included in the original experimental panel.

Computational gene imputation offers a potential solution by predicting the spatial expression of genes that were not directly measured.

This project investigates the feasibility of integrating publicly available colorectal cancer scRNA-seq data with targeted Xenium spatial transcriptomics to predict unmeasured gene expression.

An established spatial gene-imputation method will be evaluated using a held-out gene validation strategy, in which measured Xenium genes are temporarily excluded from the spatial input, computationally predicted, and compared with their original measurements.

The project also includes a literature review of existing spatial gene-imputation approaches to identify methodological limitations and opportunities for future machine-learning development.

---

## 2. Research Objectives

### Primary Objective

Evaluate the ability of scRNA-seq-guided spatial gene imputation to recover gene expression patterns in targeted colorectal cancer spatial transcriptomics data.

### Specific Objectives

1. Acquire and preprocess publicly available colorectal cancer scRNA-seq and Xenium spatial transcriptomics datasets.
2. Perform cell-level and gene-level quality control.
3. Harmonize gene identifiers and identify shared genes across datasets.
4. Implement an established spatial gene-imputation method.
5. Evaluate prediction accuracy using measured Xenium genes withheld during model input preparation.
6. Investigate associations between gene expression characteristics and imputation performance.
7. Identify limitations of existing approaches that may inform future methodological development.

### Research Question

**Can scRNA-seq-guided gene imputation accurately reconstruct the spatial expression of genes withheld from a targeted Xenium transcriptomics panel?**

---

## 3. Datasets

### 3.1. Single-Cell RNA Sequencing

**Source:** NCBI Gene Expression Omnibus (GEO)  
**Accession:** [GSE289314](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE289314)  
**Tissue:** Human colorectal cancer and associated tissues  
**Data Type:** scRNA-seq expression matrices

The downloaded dataset contains 14 samples representing colorectal tumor, paracancerous colon tissue, and liver tissue.

Sample-specific expression matrices were imported and combined into AnnData objects with associated patient, tissue, and experimental condition metadata.

Because the supplied matrices contain noninteger expression values, they were treated as previously processed expression data rather than raw UMI counts. Secondary quality control was performed without applying additional library-size normalization or logarithmic transformation.

A colorectal tumor reference was constructed using samples from Patients 2, 4, and 6.

**Final Tumor Reference:**

| Characteristic | Value |
|---|---|
| Cells | 14,385 |
| Genes | 22,239 |
| Patients | 3 |
| Tissue | Colon tumor |
| Storage Format | AnnData (.h5ad) |

A separate paracancerous reference was also generated for potential comparative analyses.

**Note:** The selected patients corresponded to an earlier GEO spatial dataset considered during project development. The current public Xenium dataset is not assumed to be patient-matched to this scRNA-seq reference.

### 3.2. Xenium Spatial Transcriptomics

**Source:** 10x Genomics Public Datasets  
**Dataset:** Xenium Human Colon Cancer  
**Technology:** Xenium targeted in situ gene expression  
**Data Type:** Cell-resolved spatial transcriptomics

The dataset includes a cell-by-gene expression matrix, cell segmentation information, spatial coordinates, transcript-level data, and associated imaging files.

**Dataset Characteristics:**

| Characteristic | Value |
|---|---|
| Original Cells | 647,524 |
| Cells After Preliminary QC | 627,550 |
| Cells Removed | 19,974 |
| Cell Retention | 96.92% |
| Biological Genes | 325 |
| Spatial Coordinate Units | Micrometers |
| Storage Format | AnnData (.h5ad) |

The Xenium dataset serves as the spatial reference for downstream gene-imputation experiments.

---

## 4. Computational Workflow

The computational workflow consists of six primary stages.

### Stage 1: Data Acquisition and Import — Completed

- Download scRNA-seq and Xenium datasets.
- Import expression matrices into Python.
- Construct AnnData objects.
- Attach sample and cell metadata.
- Verify unique identifiers and data integrity.

### Stage 2: scRNA-seq Quality Control — Completed

- Inspect expression matrix structure and numerical properties.
- Verify cell and gene identifiers.
- Evaluate missing, nonfinite, and negative expression values.
- Calculate gene detection and expression characteristics.
- Assess mitochondrial and ribosomal expression fractions.
- Generate a colorectal tumor reference.
- Save processed AnnData files.

### Stage 3: Xenium Quality Control — Completed (Preliminary)

- Import cell-level Xenium expression data.
- Extract cell identifiers and spatial metadata.
- Validate alignment between expression and segmentation records.
- Attach spatial centroid coordinates.
- Calculate transcripts and detected genes per cell.
- Evaluate candidate cell-filtering thresholds.
- Examine the spatial distribution of excluded cells.
- Calculate gene-level transcript abundance and detection frequencies.
- Save the preliminary QC-filtered dataset and summary tables.

Preliminary filtering criteria required:

- More than 10 biological transcripts per cell.
- More than 5 detected biological genes per cell.

These thresholds may be revisited following additional sensitivity assessment.

### Stage 4: Gene Harmonization — Planned

- Standardize gene identifiers across platforms.
- Identify shared genes.
- Characterize gene overlap.
- Examine gene detection and expression characteristics.
- Prepare input matrices for imputation.

### Stage 5: Spatial Gene Imputation — Planned

- Select an established imputation method.
- Construct training and held-out gene sets.
- Remove held-out genes from the spatial model input.
- Predict expression using the scRNA-seq reference and the selected method.
- Preserve original Xenium measurements for independent evaluation.

### Stage 6: Performance Evaluation — Planned

Prediction accuracy will be evaluated using:

- Pearson correlation coefficient
- Spearman correlation coefficient
- Mean absolute error (MAE)
- Root mean squared error (RMSE)

Additional analyses will examine variation in prediction performance across genes with different expression abundances, detection frequencies, and spatial expression patterns.

Observed and predicted spatial expression maps will be compared to assess whether imputation preserves biologically relevant spatial distributions.

---

## 5. Repository Structure

```text
crc_spatial_transcriptomics/
│
├── data/
│   ├── raw/
│   │   ├── scrna/
│   │   └── spatial/
│   ├── processed/
│   └── metadata/
│
├── notebooks/
│   ├── 00_environment_test.ipynb
│   ├── 01_scrna_qc.ipynb
│   ├── 02_xenium_qc.ipynb
│   └── 03_gene_harmonization.ipynb 
│
├── src/
│
├── results/
│   ├── figures/
│   └── tables/
│
├── manuscript/
│
├── presentation/
│
├── docs/
│
├── environment.yml
├── .gitignore
└── README.md
```

### Directory Descriptions

| Directory | Purpose |
|---|---|
| `data/raw/` | Original downloaded datasets |
| `data/processed/` | Processed AnnData objects |
| `data/metadata/` | Sample and dataset metadata |
| `notebooks/` | Computational analyses |
| `src/` | Reusable Python functions and scripts |
| `results/figures/` | Generated visualizations |
| `results/tables/` | QC summaries and analysis results |
| `manuscript/` | Manuscript drafts and supporting material |
| `presentation/` | Presentation materials |
| `docs/` | Additional documentation |

Large datasets and processed AnnData files are excluded from version control.

---

## 6. Completed Computational Analyses

### 6.1. scRNA-seq Preprocessing

**Notebook:** `01_scrna_qc.ipynb`

Completed analyses include:

- Importing all 14 scRNA-seq samples.
- Creating sample and patient metadata.
- Identifying previously processed expression matrices.
- Performing secondary cell-level and gene-level quality assessment.
- Evaluating mitochondrial and ribosomal expression fractions.
- Selecting colorectal tumor samples from Patients 2, 4, and 6.
- Generating tumor and paracancerous reference datasets.

**Primary Outputs:**

```text
data/processed/GSE289314_matched_tumor_reference.h5ad
data/processed/GSE289314_matched_paracancerous_reference.h5ad
```

### 6.2. Xenium Preprocessing

**Notebook:** `02_xenium_qc.ipynb`

Completed analyses include:

- Importing the Xenium feature matrix.
- Constructing a cell-by-gene AnnData object.
- Validating cell identifiers against segmentation records.
- Extracting and validating spatial coordinates.
- Calculating cell-level QC metrics.
- Evaluating filtering thresholds.
- Assessing the spatial distribution of removed cells.
- Applying preliminary QC filtering.
- Characterizing gene-level expression and detection.

**Primary Outputs:**

```text
data/processed/xenium_colon_qc_filtered.h5ad
results/tables/xenium_gene_qc_metrics.csv
results/tables/xenium_qc_summary.csv
```

---

## 7. Software and Computational Environment

Analyses are performed in Python using a Conda environment named `crc_spatial`.

### Principal Libraries

| Library | Purpose |
|---|---|
| Scanpy | Single-cell data analysis |
| AnnData | Annotated expression matrices |
| NumPy | Numerical computation |
| pandas | Data manipulation and metadata |
| SciPy | Sparse matrices and scientific computation |
| Matplotlib | Data visualization |
| Zarr | Reading Xenium data archives |
| Jupyter | Interactive computational notebooks |

The project environment is documented in `environment.yml`.

### Environment Setup

**1. Clone the repository:**

```bash
git clone https://github.com/gracescanlan23/crc_spatial_transcriptomics.git

cd crc_spatial_transcriptomics
```

**2. Create the Conda environment:**

```bash
conda env create -f environment.yml
```

**3. Activate the environment:**

```bash
conda activate crc_spatial
```

**4. Register the Jupyter kernel if needed:**

```bash
python -m ipykernel install --user \
  --name crc_spatial \
  --display-name "Python (crc_spatial)"
```

**5. Open the repository and select the `Python (crc_spatial)` kernel.**

The raw datasets must be obtained separately because large data files are not included in the repository.

---

## 8. Reproducibility

The repository is organized to support reproducible computational analyses.

Reproducibility practices include:

- Preserving original downloaded datasets.
- Separating raw and processed data.
- Maintaining sample and patient metadata.
- Recording preprocessing and QC decisions.
- Saving intermediate AnnData objects.
- Exporting analysis summaries as CSV files.
- Organizing computational steps into numbered notebooks.
- Maintaining dependency information in `environment.yml`.

Large raw datasets and processed expression matrices are not stored directly in the GitHub repository.

The complete computational workflow will be documented as development progresses.

---

## 9. Expected Outputs

The completed project is expected to generate:

1. Quality-controlled scRNA-seq and Xenium datasets.
2. A harmonized set of genes shared across platforms.
3. An established spatial gene-imputation workflow.
4. Held-out gene predictions and corresponding ground-truth measurements.
5. Gene-level and aggregate prediction performance metrics.
6. Visual comparisons of observed and predicted spatial expression.
7. Analyses of gene characteristics associated with imputation accuracy.
8. A literature-based assessment of methodological limitations and future research opportunities.

---

## 10. Future Research Directions

This project serves as a computational foundation for a potential follow-up research study investigating machine-learning approaches for reconstructing unmeasured gene expression in legacy targeted spatial transcriptomics datasets.

The proposed future direction involves integrating:

- Transcriptome-wide scRNA-seq reference data.
- High-dimensional or whole-transcriptome spatial expression data.
- Legacy low-plex Xenium spatial transcriptomics measurements.

The objective would be to investigate whether combining these complementary data sources can improve the prediction of genes absent from older targeted spatial panels.

Potential methodological developments include incorporating spatial neighborhood information, cell-type-specific expression patterns, and learned relationships between measured and unmeasured genes.

Any proposed novel method would require benchmarking against established imputation approaches and validation using independently measured spatial gene expression.

---

## 11. Project Status

| Component | Status |
|---|---|
| Computational environment | Completed |
| scRNA-seq acquisition | Completed |
| scRNA-seq secondary QC | Completed |
| scRNA-seq reference construction | Completed |
| Xenium acquisition | Completed |
| Xenium cell-level QC | Completed (preliminary) |
| Xenium gene-level QC | Completed |
| Spatial coordinate validation | Completed |
| Gene harmonization | Planned |
| Imputation method selection | Planned |
| Held-out gene validation | Planned |
| Performance evaluation | Planned |
| Final figures and manuscript | In progress |

**Last Updated:** October 13 2026

---

## 12. Data Availability

The scRNA-seq dataset is publicly available through NCBI GEO under accession [GSE289314](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE289314).

The Xenium human colon cancer dataset was obtained from the [10x Genomics datasets portal](https://www.10xgenomics.com/datasets).

Raw data files and large processed AnnData objects are excluded from this repository due to their size.

---

## 13. Author

**Grace Scanlan**  
Master's Program in Bioinformatics / Applied Biomedical Data Science  
Meharry Medical College May 2027

GitHub: [gracescanlan23](https://github.com/gracescanlan23)

---

## 14. Project Disclaimer

This repository contains an academic computational research project under active development. The current preprocessing workflow and quality control procedures are documented for research and educational purposes.

Gene-imputation experiments and performance evaluations have not yet been completed. Results and methodological decisions may be updated as additional analyses are performed.

## Status
Project initiated August 2026.
Final project due November 2026.