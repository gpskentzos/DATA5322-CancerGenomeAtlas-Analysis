# Can an Algorithm Rediscover What Oncologists Took Decades to Find?

*A data science team at Seattle University applied unsupervised machine learning to breast cancer gene profiles — without giving the algorithm a single label.*

---

## The Question

Breast cancer is not one disease. Oncologists have identified at least four distinct molecular subtypes — **Luminal A**, **Luminal B**, **HER2-enriched**, and **Triple-Negative** — each with different behaviors, prognoses, and treatment responses. Identifying these subtypes took decades of clinical research and required knowing exactly what to look for.

We asked a simpler question: **what if you gave an algorithm 17,000 gene measurements from 529 tumor samples and absolutely no other information? Could it find the same groupings on its own?**

---

## The Data

We used publicly available gene expression data from **The Cancer Genome Atlas (TCGA)**, a landmark NIH project that catalogued the genomics of human cancer at an unprecedented scale. Our dataset contains 529 breast tumor samples, each profiled across 17,814 genes. Every entry is a number representing how actively that gene was expressing itself in that tumor.

Each patient is described by 17,814 continuous numerical gene expression measurements. The high dimensionality is intentional. Our analysis uses PCA to reduce it to a a manageable number of dimensions of components before clustering, which is the standard approach in genomics. Our job was to find hidden patterns in those numbers, with no hints about diagnosis, subtype, or outcome.

> **Data source:** [Kaggle — Gene Expression Profiles of Breast Cancer](https://www.kaggle.com/datasets/orvile/gene-expression-profiles-of-breast-cancer) (originally from TCGA)

---

## Our Approach

We applied four unsupervised learning methods, which are algorithms that learn structure from data without any labels:

| Method | What it does |
|--------|-------------|
| **PCA / SVD** | Compresses 17,000 dimensions down to a handful that capture the most variation |
| **Matrix Completion** | Recovers missing measurements using the low-rank structure of the data |
| **K-Means Clustering** | Partitions patients into groups based on similarity in gene expression |
| **Hierarchical Clustering** | Builds a tree of relationships between patients, revealing nested structure |

---

## What We Found

### Dimensionality Reduction (PCA)



Even with 17,000 genes, a surprisingly small number of dimensions capture most of the variation. The first two principal components alone reveal...


### Handling Missing Data (Matrix Completion)


Gene expression data collected across different labs and platforms often contains gaps. We demonstrated that...


### K-Means Clustering

> *[FILL IN: Describe the elbow plot result, silhouette score at k=4, cluster sizes, and what the clusters look like in PCA space. Include the elbow plot and PCA scatter.]*

When we asked the algorithm to find four natural groupings (matching the four known subtypes), we found...

<!-- ![Elbow Plot](../plots/kmeans_elbow.png) -->
<!-- ![K-Means Clusters](../plots/kmeans_clusters_pca.png) -->

### Hierarchical Clustering


Building a family tree of the tumor samples revealed...


---

## The Bigger Picture


---

## What This Means

This project is a proof of concept for something profound: the biological structure of breast cancer is so deeply encoded in gene expression that an algorithm with zero clinical knowledge can begin to find it.

This matters for a few reasons:

- In cancers or diseases where subtypes aren't yet well-characterized, unsupervised methods like these can generate hypotheses for future research.
- As genomic profiling becomes cheaper and more routine, these methods will help make sense of massive datasets that no human team could review manually.
- It raises an interesting question about what "knowledge" really is — and how much structure in biology is waiting to be found by the right mathematical lens.

---

## About This Project

This analysis was completed as a final project for **DATA 5322 — Statistical Machine Learning II** at Seattle University, Spring 2026.

**Team:** Ruman Sidhu, Paul Skentzos, Hamda Hassan  
**Instructor:** Dr. Ariana Mendible  
**Code & notebooks:** [GitHub Repository](https://github.com/[YOUR-REPO-LINK])

---

*All analysis was performed in Python using scikit-learn, scipy, pandas, matplotlib, and seaborn. Data is from the public domain via TCGA and Kaggle.*
