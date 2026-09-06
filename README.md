# Benchmarking Spatial Gene-Expression Imputation Methods Through Integration of Single-Cell and Spatial Transcriptomics in Colorectal Cancer
## Overview
## Research Question
## Specific Aims
## Workflow
### scRNA-seq
- Quality control
- Normalization
- Highly variable gene selection
- PCA
- Neighborhood graph
- Leiden Clustering
- UMAP
- Marker gene identification
- Cell-type annotation

### Spatial Transcriptomics
- Quality control
- Normalization
- Spatial domain identification
- Spatially variable gene analysis
- Spatial visualization

### Integration

### Downstream Analysis

## Project Structure
```text 
data/
    raw/
    processed/
    metadata/

notebooks/
src/

results/
    figures/
    tables/

manuscript/
presentation/
```
## Set up Python Environment

Check whether Conda is installed:

conda --version

Create an environment:
conda create -n crc_spatial python=3.13

Activate it:

conda activate crc_spatial

Install packages using conda install:

- scanpy
- jupyterlab
- ipykernel
- pandas
- numpy
- matplotlib
- scipy
- scikit-learn
- squidpy


## Status
Project initiated August 2026.
Final project due November 2026.

