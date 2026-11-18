# Module 15: Unsupervised Learning

Most data has no labels. Nobody has marked which customers belong together, which transactions are suspicious, or which of 64 pixel values matter. Unsupervised learning looks for structure on its own: **groups** (clustering), **compact descriptions** (dimensionality reduction) and **unusual cases** (anomaly detection). With no answer key, checking whether that structure is real is a central part of the work, and this module treats it that way.

## Notebooks

| # | Notebook | What's in it | Open |
|---|----------|--------------|------|
| 1 | Lesson | k-means by hand, choosing k, scaling, Gaussian mixtures, hierarchical clustering, DBSCAN, PCA by hand, t-SNE and UMAP, anomaly detection, checking whether structure is real | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/15-unsupervised-learning/01_lesson.ipynb) |
| 2 | Exercises | 12 exercises, from the two steps of k-means to DBSCAN and the EM algorithm written from scratch | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/15-unsupervised-learning/02_exercises.ipynb) |
| 3 | Solutions | Worked solutions, with the idea behind each and the mistake each check catches | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/15-unsupervised-learning/03_solutions.ipynb) |
| 4 | Mini project | Customer groups for a wholesale distributor: PCA, outliers, choosing and testing clusters, and an honest report | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/anupbagale/python-for-ai-ml/blob/main/15-unsupervised-learning/04_mini_project.ipynb) |

There's also a [cheatsheet](cheatsheet.md) for quick revision. The wholesale data and its source are described in [data/README.md](data/README.md).

## What you'll learn

**k-means, built by hand.** Lloyd's two steps, inertia, local minima, k-means++ and several starts.

**How many clusters?** The elbow and the silhouette, and a warning shown in pictures: clustering always finds clusters, even in random points.

**Scaling decides similarity.** Penguins clustered with and without standardisation, checked against the species with the adjusted Rand index.

**Beyond round clusters.** Gaussian mixtures with soft assignments and BIC; hierarchical clustering, dendrograms and the four linkages; DBSCAN, choosing `eps`, and HDBSCAN.

**PCA, built by hand.** The SVD of centred data, loadings, explained variance, and handwritten digits rebuilt from a few components.

**Maps of high-dimensional data.** t-SNE turns 64-pixel digits into ten islands without seeing a label; what such maps can and can't tell you; UMAP.

**Anomaly detection.** Robust z-scores, Isolation Forest and Local Outlier Factor, tested on data with known anomalies, including the ones no method can find.

**Is the structure real?** Internal measures, external labels, stability under resampling compared with random data, and usefulness.

## Exercises

| Level | Exercises |
|-------|-----------|
| Easy | `assign_to_nearest`, `inertia`, `robust_z_scores`, `cluster_profile` |
| Medium | `kmeans`, `kmeans_plus_plus`, `silhouette_samples_manual`, `pca_svd`, `adjusted_rand_index` |
| Hard | `dbscan`, `single_linkage_clusters`, `gaussian_mixture_1d` |

Most checks compare your code with scikit-learn's version of the same method. Others catch the classic mistakes:

- PCA without centring
- a silhouette that counts a point's distance to itself
- DBSCAN that explores border points and leaks across gaps
- an EM step that uses the old means for the new variances

## Mini project

Annual spending of 440 customers of a Portuguese wholesale distributor (CC BY 4.0):

- **Transform and map.** Logs tame the skew; PCA reduces six categories to a retail-basket axis and a kitchen-basket axis, holding about 70% of the variation.
- **Unusual customers.** Three methods agree on customers who bought almost nothing in some categories. They're kept, reported, and shown not to change the result.
- **How many groups?** Inertia, silhouette, BIC and bootstrap stability give a mixed picture. Two groups are clearly the most stable, but the silhouette is low.
- **Two groups.** A "shop-like" and a "kitchen-like" group, described in the original units, and found from spending alone. They match the recorded channel well (ARI 0.52), and have nothing to do with region.
- **Honest limits.** Other algorithms split the customers differently and the maps show one continuous cloud, so finer segments are presented as convenient divisions, not natural types.

## Prerequisites

Modules [1](../01-python-foundations) to [14](../14-trees-and-ensembles). Module 6 (NumPy broadcasting), Module 9 (linear algebra, the SVD and the bootstrap) and Module 13 (thresholds for anomaly scores) are used most.

## Next

[Module 16: Time Series and Recommendations](../16-time-series-and-recommendations)
