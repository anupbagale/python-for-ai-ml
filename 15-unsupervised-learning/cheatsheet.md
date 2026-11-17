# Module 15 Cheatsheet

## Three jobs

| job | question | main tools |
|---|---|---|
| clustering | which examples belong together? | k-means, Gaussian mixtures, hierarchical, DBSCAN/HDBSCAN |
| dimensionality reduction | can fewer numbers describe the data? | PCA, t-SNE, UMAP |
| anomaly detection | which examples don't fit? | robust z-scores, Isolation Forest, LOF |

No labels means no answer key: always check whether the structure is real.

## Prepare the features

```python
Z = StandardScaler().fit_transform(np.log(X))      # logs for skewed amounts, then standardise
```

Feature choice and scaling **define** similarity. Unscaled, the feature with the largest units dominates every distance.

## k-means

```python
from sklearn.cluster import KMeans
km = KMeans(n_clusters=4, n_init=10, random_state=0).fit(Z)
km.labels_, km.cluster_centers_, km.inertia_
```

- Lloyd's algorithm: **assign** each point to the nearest centroid, **update** each centroid to the mean of its points, repeat.
- Minimises **inertia** (total squared distance to the own centroid). Each step can only lower it.
- Local minima: use k-means++ starts (the default) and `n_init` several runs.
- Assumes round clusters of similar size.

## Choosing k

```python
from sklearn.metrics import silhouette_score
silhouette_score(Z, labels)            # -1 to 1; below about 0.25 to 0.3 is weak structure
```

- **Elbow**: inertia always falls; look for where it stops falling fast.
- **Silhouette**: `s = (b - a) / max(a, b)`, with `a` the mean distance within the own cluster and `b` to the nearest other cluster.
- **BIC** for Gaussian mixtures (lower is better).
- **Stability**: bootstrap, recluster, compare with the ARI; compare against random data of the same spread.
- Clustering always returns clusters, even for random data. The data may have no single right k.

## Comparing clusterings

```python
from sklearn.metrics import adjusted_rand_score
adjusted_rand_score(labels_a, labels_b)     # 1 = same grouping (names ignored), about 0 = unrelated
```

## Gaussian mixtures

```python
from sklearn.mixture import GaussianMixture
gm = GaussianMixture(n_components=3, covariance_type="full", n_init=5, random_state=0).fit(Z)
gm.predict(Z), gm.predict_proba(Z), gm.bic(Z)
```

Elliptical clusters, soft assignments. Fitted by **EM**: E-step responsibilities, M-step weighted means, variances and shares. Use `"diag"` or `"spherical"` covariances with many features and little data.

## Hierarchical clustering

```python
from scipy.cluster.hierarchy import linkage, dendrogram
dendrogram(linkage(Z, method="ward"))
from sklearn.cluster import AgglomerativeClustering
AgglomerativeClustering(n_clusters=3, linkage="ward").fit_predict(Z)
```

| linkage | cluster distance | finds |
|---|---|---|
| single | closest pair | chains, odd shapes; sensitive to noise bridges |
| complete | farthest pair | compact, similar diameters |
| average | mean over pairs | compromise; can split off a few outliers |
| Ward | increase in variance | compact, similar sizes (like k-means) |

Memory grows with n²: up to tens of thousands of points.

## DBSCAN and HDBSCAN

```python
from sklearn.cluster import DBSCAN, HDBSCAN
DBSCAN(eps=0.15, min_samples=5).fit_predict(Z)      # -1 = noise
HDBSCAN(min_cluster_size=10).fit_predict(Z)
```

- Core point: at least `min_samples` neighbours within `eps` (itself included). Clusters grow through core points; border points join; the rest is noise.
- Choose `eps` from the bend in the sorted distances to the `min_samples`-th nearest neighbour.
- Any shape, finds k itself, marks noise. Struggles with varying density: use HDBSCAN.

## PCA

```python
from sklearn.decomposition import PCA
pca = PCA(n_components=2).fit(Z)
pca.components_, pca.explained_variance_ratio_, pca.transform(Z), pca.inverse_transform(scores)
```

- By hand: centre, `U, S, Vt = np.linalg.svd(X - X.mean(0))`; components = rows of `Vt`; variance = `S² / (n - 1)`.
- **Always centre**; usually standardise first.
- Loadings explain what each component means. Signs are arbitrary.
- Linear only. Uses: visualisation, compression, noise removal, understanding correlations.

## t-SNE and UMAP

```python
from sklearn.manifold import TSNE
TSNE(n_components=2, perplexity=30, init="pca", random_state=0).fit_transform(Z)
# pip install umap-learn; umap.UMAP(n_neighbors=15, min_dist=0.1).fit_transform(Z)
```

Keep neighbours together. Distances between clusters and cluster sizes on the map mean little; results depend on settings and seed. For looking, not modelling.

## Anomaly detection

```python
z = (x - np.median(x)) / (1.4826 * np.median(np.abs(x - np.median(x))))     # robust z; flag |z| > 3.5
from sklearn.ensemble import IsolationForest
from sklearn.neighbors import LocalOutlierFactor
score_iso = -IsolationForest(random_state=0).fit(Z).score_samples(Z)                     # higher = more unusual
score_lof = -LocalOutlierFactor(n_neighbors=20).fit(Z).negative_outlier_factor_
```

- Isolation Forest: unusual points are isolated by few random splits. LOF: lower density than the neighbours.
- `contamination` only sets the threshold. How many to flag is a cost and capacity decision.
- An anomaly is a question, not an error. Some anomalies look normal and can't be found.

## Is the structure real?

1. Internal measures (silhouette, inertia, BIC): useful, but random data scores too.
2. External labels where available (ARI).
3. Stability under resampling, compared with random data.
4. Agreement between different methods.
5. Usefulness: does it improve decisions? Test it.

Describe clusters in **original units** (sizes and medians), and report what the analysis doesn't support.
