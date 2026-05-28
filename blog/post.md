# Can an Algorithm Rediscover What Oncologists Took Decades to Find?

---

## The Question

Breast cancer is not one disease. Oncologists have identified at least four distinct molecular subtypes: **Luminal A**, **Luminal B**, **HER2-enriched**, and **Triple-Negative**. Each behaves differently, responds differently to treatment, and carries a different prognosis. Identifying these subtypes took decades of clinical research and required knowing exactly what to look for.

We asked a simpler question: **what if you gave an algorithm 17,000 gene measurements from 529 tumor samples and absolutely no other information? Could it find the same groupings on its own?**

---

## The Data

We used publicly available gene expression data from **The Cancer Genome Atlas (TCGA)**, a landmark NIH project that cataloged the genomics of human cancer at an unprecedented scale. Our dataset contains 529 breast tumor samples, each profiled across 17,814 genes. Every entry is a number representing how actively that gene was expressing itself in that tumor.

Each patient is described by 17,814 continuous numerical gene expression measurements. The high dimensionality is intentional. Our analysis uses PCA to reduce it to a manageable number of dimensions before clustering, which is the standard approach in genomics. Our job was to find hidden patterns in those numbers, with no hints about diagnosis, subtype, or outcome.

### A note on the numbers themselves

The values in this dataset are not raw counts. They are log-transformed measurements, already centered near zero. That matters for the analysis. Gene activity spans several orders of magnitude across a dataset like this, and working directly with raw values would let a small number of extremely active genes dominate the results. The log transformation compresses that range, puts all measurements on a more comparable scale, and makes the data behave well for the mathematical tools we apply.

### What the data inspection revealed

When we examined the dataset carefully, we found 1,497 entries out of 9.4 million recorded as missing, less than 0.02% of the data. The missing values were not distributed randomly. They clustered in a specific subset of features that showed consistently undetectable activity across all 529 samples. The measurement platform records no result rather than a near-zero number when the signal falls below its detection threshold, because a number below that threshold would be less accurate than recording nothing at all.

This kind of structured missingness, where the pattern of what is missing carries information about the data, is a recurring challenge in high-dimensional scientific datasets. It directly motivates one of our methods.

Before running any models, we also filtered the dataset from 17,814 genes down to the top 5,000 by how much they varied across patients. Those 5,000 genes account for 64.4% of the total variation in the dataset. The remaining 12,814 genes contribute very little signal and would mostly add noise to the analysis.

> **Data source:** [Kaggle: Gene Expression Profiles of Breast Cancer](https://www.kaggle.com/datasets/orvile/gene-expression-profiles-of-breast-cancer) (originally from TCGA)

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

<!-- ![Scree Plot](../plots/scree_plot.png) -->
<!-- ![PCA Biplot](../plots/pca_biplot.png) -->

---

### Handling Missing Data (Matrix Completion)

The 1,497 missing values in the raw data raised a natural question: if the data has gaps, can mathematics fill them in reliably?

To test this properly, we ran a controlled experiment. We took the clean, complete expression matrix and deliberately hid 10% of entries, about 264,000 values chosen at random. We then applied Iterative SVD Completion, an algorithm that exploits the structure of the data to predict what those hidden values should be.

**Why does this work?** The 529 patients in this dataset do not vary independently across 5,000 genes. Patients with similar overall expression profiles tend to show correlated patterns across many genes at once. This creates redundancy in the data matrix: a relatively small number of underlying patterns explain most of what we observe. Once the algorithm learns those patterns from the observed entries, it can predict the missing ones.

We measured how well the algorithm recovered the hidden values across a range of model complexities, expressed as a rank parameter:

<!-- ![RMSE vs Rank](../plots/mc_rmse_vs_rank.png) -->

The results showed a clear sweet spot. Using rank 50 produced the best recovery:

- **RMSE of 0.72** on held-out entries, compared to 1.00 for simply predicting the mean (a 28% improvement)
- **R² of 0.48**: the model explains about half the variance in the missing values

Going beyond rank 50 made things slightly worse. The model began fitting noise in the observed entries rather than genuine signal, which hurt its predictions on the hidden ones. This is the classic bias-variance tradeoff.

**What does rank 50 tell us about the data?** The four known subtypes might lead you to expect an optimal rank of around 4. The higher value reflects additional structure in the data: variation that exists across the 529 samples beyond what separates the four primary groups. Each additional dimension of structure in the data adds to the optimal rank.

At the optimal rank of 50, the algorithm converged in 33 iterations. The Pearson correlation between predicted and true held-out values was 0.70, indicating a meaningful but not perfect recovery, which is expected for noisy high-dimensional data.

<!-- ![Convergence](../plots/mc_convergence.png) -->
<!-- ![Recovery Heatmap](../plots/mc_recovery_heatmap.png) -->

---

### K-Means Clustering


When we asked the algorithm to find four natural groupings (matching the four known subtypes), we found...

<!-- ![Elbow Plot](../plots/kmeans_elbow.png) -->
<!-- ![K-Means Clusters](../plots/kmeans_clusters_pca.png) -->

---

### Hierarchical Clustering


Building a family tree of the tumor samples revealed...

<!-- ![Dendrograms](../plots/dendrograms.png) -->
<!-- ![Expression Heatmap](../plots/heatmap_hierarchical.png) -->

---

## The Bigger Picture



---

## What This Means

This project is a proof of concept for something significant: the structure of breast cancer subtypes is so consistently encoded in gene expression measurements that an algorithm with no clinical knowledge can begin to find it from numbers alone.

This matters for a few reasons:

- In cancers or diseases where subtypes are not yet well-characterized, unsupervised methods like these can generate hypotheses for future research.
- As genomic profiling becomes cheaper and more routine, these methods will help make sense of massive datasets that no human team could review manually.
- It raises an interesting question about what knowledge really is, and how much structure is waiting to be found by the right mathematical lens.

---

## About This Project

This analysis was completed as a final project for **DATA 5322 - Statistical Machine Learning II** at Seattle University, Spring 2026.

**Team:** Ruman Sidhu, Paul Skentzos, Hamda Hassan  
**Instructor:** Dr. Ariana Mendible  
**Code & notebooks:** [GitHub Repository](https://github.com/[YOUR-REPO-LINK])

---

*All analysis was performed in Python using scikit-learn, scipy, pandas, matplotlib, and seaborn. Data is from the public domain via TCGA and Kaggle.*
