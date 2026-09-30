---
cover: ../../../.gitbook/assets/dist_sim.png
coverY: -125.19051262433052
layout:
  width: default
  cover:
    visible: true
    size: full
    mask: none
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
  anchors:
    visible: true
---

# Distance Similarity

## Distance Similarity SPIs Overview

Distance-based similarity measures aim to establish similarity or independence based on the pairwise distance between bivariate observations. All of the methods presented in this section are implemented using one of the following toolboxes:

* For distance correlation (and related) statistics, as well as reproducing kernel Hilbert space (RKHS)-based statistics, we use the [_Hypothesis Testing in Python_](https://github.com/neurodata/hyppo) (hyppo) package.
* For dynamic time warping (and related) statistics, we use the [_tslearn_](https://github.com/tslearn-team/tslearn) package.

***

> ## Barycenter
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `bary`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://lig-membres.imag.fr/bisson/cours/M2INFO-AIW-ML/papers/PetitJean11.pdf), [_\[2\]_](https://arxiv.org/abs/1701.06393)

A barycenter (or Fréchet mean) is a (univariate) time series that minimizes the sum of squared distances between MTS. The [_tslearn_](https://github.com/tslearn-team/tslearn) package provides functions to obtain barycenters by minimising the sum-of-squares of the following distance metrics:

* (Unwarped) Euclidean distance (`euclidean`).
* Alignment via expectation maximisation (`dtw`).
* Alignment via a differential loss function (`softdtw`).
* Alignment via a sub-gradient descent algorithm (`sgddtw`).

For each pair of (bivariate) time series in an M-variate MTS, we compute the raw barycenters then summarise them by taking their mean (modifier `mean`), maximum (`max`) or timepoint at which the maximum occurs (`max_time`).

<details>

<summary>Barycenter Estimators</summary>

* `bary_euclidean_mean`
* `bary_euclidean_max`
* `bary_euclidean_max_time`
* `bary_dtw_mean`
* `bary_dtw_max`
* `bary_dtw_max_time`
* `bary_softdtw_mean`
* `bary_softdtw_max`
* `bary_softdtw_max_time`
* `bary_sgddtw_mean`
* `bary_sgddtw_max`
* `bary_sgddtw_max_time`

</details>

***

> ## Cross Distance Correlation
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `dcorrx`
>
> _<mark style="color:red;">Key References</mark>_<mark style="color:red;">:</mark> [_\[1\]_](https://arxiv.org/abs/1907.02088)

The cross-distance correlation quantifies the independence between two univariate time series based on distance correlation. This measure is the average of lagged distance correlations between the past of time series `x` to the future of time series `y`, up to a given lag. We include both a low-order (`lag-1`, `maxlag-1`) and a high-order (`lag-10`, `maxlag-10`) assumption of the number of relevant lags.

<details>

<summary>Cross Distance Correlation Estimators</summary>

* `dcorrx_maxlag-1`
* `dcorrx_maxlag-10`&#x20;

</details>

***

> ## **Cross Multiscale Graph Correlation**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `mgcx`
>
> _<mark style="color:red;">Key References</mark>_<mark style="color:red;">:</mark> [_\[1\]_](https://arxiv.org/abs/1907.02088)

The cross-multiscale graph correlation is defined similarly to Cross Distance Correlation (`dcorrx`) but uses lagged MGCs instead of lagged distance correlations. We include both a low-order (`lag-1`, `maxlag-1`) and a high-order (`lag-10`, `maxlag-10`) assumption of the number of relevant lags.

<details>

<summary>Cross Multiscale Graph Correlation Estimators</summary>

* `mgcx_maxlag-1`
* `mgcx_maxlag-10`

</details>

***

> ## Distance Correlation
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `dcorr`
>
> _<mark style="color:red;">Key References</mark>_:[ _\[1\]_](https://arxiv.org/pdf/0803.4101)

Distance correlation is used to infer the independence between two random variables via pairwise distance metrics and hypothesis tests. The sample distance correlation is computed by summing over the entry-wise product of Euclidean distance matrices. This statistic is biased, however an unbiased estimator can be obtained by first double-centering the distance matrices. Although any pairwise distance metric can be used, here we only use Euclidean distance because distance correlation computed with this metric has been shown to be universally consistent (asymptotically converging to the true value). We compute both the biased and unbiased statistics from the [_hyppo_](https://github.com/neurodata/hyppo) package.

<details>

<summary>Distance Correlation Estimators</summary>

* `dcorr`
* `dcorr_biased`

</details>

***

> ## Dynamic Time Warping
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `dtw`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://ieeexplore.ieee.org/document/1163055)

Dynamic time warping (DTW) extends the ideas of measuring the pairwise Euclidean distance between time series by allowing for potentially dilated time series of variable size. Specifically, DTW finds the minimum distance between two time series through alignment (shifting and dilating of the sequences). This algorithm (and many outlined below) also includes the Sakoe-Chiba band and the Itakura parallelogram global constraints on the alignments to prevent pathological warpings. We compute this statistic using the [_tslearn_](https://github.com/tslearn-team/tslearn) package for the three global constraints, given by each of the estimators:

<details>

<summary>Dynamic Time Warping Estimators</summary>

* `dtw`
* `dtw_constraint-itakura`
* `dtw_constraint-sakoe_chiba`

</details>

***

> ## Gromov-Wasserstein Distance
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, unordered, distance.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `gwtau`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://link.springer.com/article/10.1007/s11538-023-01175-y)

The Gromov-Wasserstein distance (GWτ) represents each time series as a metric space and computing the distances from the start of each time series to every point. These distance distributions are then compared using the [Wasserstein](https://en.wikipedia.org/wiki/Wasserstein_metric) distance, which finds the optimal way to match the distances between two time series, making it robust to shifts and perturbations. The "tau" in GWτ emphasises that this distance measure is based on comparing the distributions of distances from the root (i.e., the starting point) to all other points in each time series, which is analogous to comparing the branch lengths in two tree-like structures. This SPI is based on the algorithm proposed by [Kravtsova et al. (2023)](https://doi.org/10.1007/s11538-023-01175-y).

<details>

<summary>Gromov-Wasserstein Distance Estimator</summary>

* `gwtau`

</details>

***

> ## Heller-Heller-Gorfine Independence Criterion
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `hhg`
>
> _<mark style="color:red;">Key References</mark>_<mark style="color:red;">:</mark> [_\[1\]_](https://academic.oup.com/biomet/article-abstract/100/2/503/202568?redirectedFrom=fulltext)

The Heller-Heller-Gorfine (HHG) method yields an RKHS-based statistic that uses the ranks of random variables to obtain sample kernel matrices, rather than their distances. This SPI is computed via the [_hyppo_](https://github.com/neurodata/hyppo) package.

<details>

<summary>Heller-Heller-Gorfine Independence Criterion Estimator</summary>

* `hhg`

</details>

***

> ## Hilbert-Schmidt Independence Criterion
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `hsic`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://proceedings.neurips.cc/paper_files/paper/2007/file/d5cfead94f5350c12c322b5b664544c1-Paper.pdf)

The Hilbert-Schmidt Independence Criterion (HSIC) is an RKHS-based statistic that quantifies a statistical dependence between random variables via a sample kernel matrix. As with distance correlation, HSIC yields a biased statistic, where the unbiased (and consistent) estimator can be derived by double centering the kernel distance matrix. Both `biased` and `unbiased` (no modifier) estimators are computed using [_hyppo_](https://github.com/neurodata/hyppo).

<details>

<summary>Hilbert-Schmidt Independence Criterion Estimators</summary>

* `hsic`
* `hsic_biased`

</details>

***

> ## Longest Common Subsequence
>
> [_<mark style="color:blue;">Keywords:</mark>_](../glossary-of-terms.md#keywords) _<mark style="color:blue;">undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `lcss`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://www.researchgate.net/publication/3943235_Discovering_similar_multidimensional_trajectories)

The longest common subsequence (LCSS) generalises ideas from DTW by measuring the similarity between continuous subsections of time series, rather than the entire time series themselves, subject to distance thresholds and alignment constraints. We use the default threshold from the [_tslearn_](https://github.com/tslearn-team/tslearn) package (ε = 1), and include the three global constraints:

* No constraints (no modifier)
* The band (modifier `sakoe_chiba`)
* The parallelogram (`itakura`)

<details>

<summary>Longest Common Subsequence Estimators</summary>

* `lcss`
* `lcss_constraint-itakura`
* `lcss_constraint-sakoe_chiba`

</details>

***

> ## **Multiscale Graph Correlation**
>
> [_<mark style="color:blue;">Keywords:</mark>_](../glossary-of-terms.md#keywords) _<mark style="color:blue;">undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `mgc`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://www.researchgate.net/publication/3943235_Discovering_similar_multidimensional_trajectories)

Multiscale graph correlation (MGC) is a generalisation of distance correlation that is designed to overcome its limitations in inferring nonlinear relationships such as circles and parabolas. Specifically, the algorithm truncates the Euclidean distance matrices (the procedure at the core of distance correlation) at an optimal threshold. MGC also includes a smoothing function that is intended to remove any bias introduced by disconnected components in the graph introduced by truncating the distance matrix. Only this unbiased estimator is included here (as in [_hyppo_](https://github.com/neurodata/hyppo)).

<details>

<summary>Multiscale Graph Correlation Estimator</summary>

* `mgc`

</details>

***

> ## **Soft Dynamic Time Warping**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `softdtw`
>
> _<mark style="color:red;">Key References</mark>:_ [_\[1\]_](https://proceedings.mlr.press/v70/cuturi17a.html)

Soft dynamic time warping uses a smoothed formulation of DTW to optimize the minimal-cost alignment as a differentiable loss function. This method is computed via the [_tslearn_](https://github.com/tslearn-team/tslearn) package (with the default hyperparameter, γ = 1), and includes the same global constraints as DTW:

* No constraints (no modifier)
* The band (modifier `sakoe_chiba`)
* The parallelogram (`itakura`)

<details>

<summary>Soft Dynamic Time Warping Estimators</summary>

* `softdtw`
* `softdtw_constraint-itakura`
* `softdtw_constraint-sakoe_chiba`

</details>



> ## **Pairwise Distance**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md)_<mark style="color:blue;">: undirected, nonlinear, unsigned, unordered.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `pdist`

Pairwise distance between time-series, computed using _sklearn's_ [pairwise distance function](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise_distances.html) using the following distance metrics:

* Euclidean
* Cityblock
* Cosine
* Chebyshev
* Canberra
* Braycurtis

<details>

<summary>Pairwise Distance Estimators</summary>

* `pdist_euclidean`
* `pdist_cityblock`
* `pdist_cosine`
* `pdist_chebyshev`
* `pdist_canberra`
* `pdist_braycurtis`

</details>
