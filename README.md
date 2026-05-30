# DATA5322 - Cancer Genome Atlas Analysis

A DATA 5322 group project analyzing cancer gene expression data using unsupervised learning methods to independently rediscover known molecular subtypes of breast cancer.

---

## Project Goal

Using gene expression profiles from breast cancer datasets, we aim to determine whether unsupervised machine learning can independently rediscover known molecular subtypes of breast cancer - Luminal A, Luminal B, HER2-enriched, and Triple-Negative - without any prior labels or clinical information.

The central question: can an algorithm that knows nothing about cancer biology find the same patient groupings that oncologists identified through decades of clinical research?

---

## Dataset

The dataset is sourced from Kaggle and is too large to host directly in this repository. Download it here:

**Kaggle (working dataset):**
https://www.kaggle.com/datasets/orvile/gene-expression-profiles-of-breast-cancer

**Official Source - The Cancer Genome Atlas (TCGA):**
https://portal.gdc.cancer.gov

Once downloaded, the dataset contains the following files:

- `BC-TCGA/` - Tumor and Normal gene expression data from The Cancer Genome Atlas
- `GSE2034/` - Tumor and Normal samples from GEO Series GSE2034
- `GSE25066/` - Tumor and Normal samples from GEO Series GSE25066
- `Simulation-Data/` - Simulated Normal and Tumor expression data for testing

Each dataset contains continuous numerical gene expression measurements across hundreds of tumor samples, making it well-suited for dimensionality reduction and clustering analysis.

---

## Methods

All four unsupervised learning methods will be applied and interpreted in the context of the data:

- **PCA / SVD** - Dimensionality reduction to identify which genes drive the most variation across tumor samples. Includes scree plot, proportion of variance explained, and biplot visualization.
- **Matrix Completion** - Handling missing gene expression values using low-rank matrix approximation.
- **K-Means Clustering** - Grouping tumor samples by similarity in gene expression profiles to discover natural patient subgroups.
- **Hierarchical Clustering** - Building a dendrogram of tumor sample relationships using multiple linkage types (complete, single, average) and comparing results.

---

## Communication Deliverable

The final deliverable includes a blog post written for a broad audience, with visualizations and interpretation of results. The blog post will be hosted on GitHub and formatted using Markdown.

---

## Repository Structure

```
DATA5322-CancerGenomeAtlas-Analysis/
│
├── data/               # Instructions for downloading the dataset
├── notebooks/          # Python Jupyter Notebooks
├── plots/              # Generated visualizations
├── blog/               # Blog post content
└── README.md
```

---

## Course

**DATA 5322** - Statistical Machine Learning II
Seattle University, Spring 2026
Instructor: Dr. Ariana Mendible

---

## Team

- Ruman Sidhu
- Paul Skentzos
- Hamda Hassan
