---
cover: >-
  https://images.unsplash.com/photo-1634117622592-114e3024ff27?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxzdGF0aXN0aWNzfGVufDB8fHx8MTcxMzkyNDc2OXww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Basic Statistics

## Basic SPIs Overview

In this section, we detail the SPIs that we have categorised as 'basic statistics'. These include SPIs that are foundational to statistical analysis, often because they are widely applicable, relatively straightforward to understand and compute, and can form the basis for more complex methods.&#x20;

***

> ## Covariance
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark><mark style="color:blue;">:</mark>_ `cov`

The covariance matrix is estimated for a wide variety of statistical procedures. Due to the _z_-scoring of the time series, the correlation and covariance matrices are equivalent and thus the covariance statistic is within \[−1, 1]. We use _scikit-learn_ to compute the covariance matrix via a number of estimators:&#x20;

* Standard maximum likelihood estimate (MLE) (denoted by the modifier [`EmpiricalCovariance`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.EmpiricalCovariance.html)).
* Elliptic envelope ([`EllipticEnvelope`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.EllipticEnvelope.html)).&#x20;
* Minimum covariance determinant ([`MinCovDet`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.MinCovDet.html)) methods for outlier removal.
* Lasso technique, which uses an l1-regularisation to sparsify the covariance matrix ([`GraphicalLasso`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.GraphicalLasso.html)).
  * A method with the regularisation method chosen through cross-validation with five splits, ([`GraphicalLassoCV`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.GraphicalLassoCV.html)).
* &#x20;Basic shrinkage covariance estimator with a fixed shrinkage coefficient of 0.1 ([`ShrunkCovariance`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.ShrunkCovariance.html)).
* The Ledoit-Wolf method for optimising the shrinkage coefficient ([`LedoitWolf`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.LedoitWolf.html)).
* Oracle approximating shrinkage, an improved method for optimising the shrinkage coefficient if the data are Gaussian ([`OAS`](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.OAS.html)).

<details>

<summary>Covariance Estimators </summary>

* `cov_EmpiricalCovariance`
* `cov_EllipticEnvelope`
* `cov_MinCovDet`
* `cov_GraphicalLasso`
* `cov_GraphicalLassoCV`
* `cov_ShrunkCovariance`
* `cov_LedoitWolf`
* `cov_OAS`

</details>

***

> ## Cross Correlation
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, linear, signed/unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark><mark style="color:blue;">:</mark>_ `xcorr`

The cross-correlation function is defined as the Pearson correlation between two time series for all lags, giving values in \[−1, 1] for each lag. To estimate the cross-correlation function, we use _SciPy_, which outputs a correlogram, i.e., the correlation from the MLE of the cross-covariance at a given lag, normalised by the auto-covariance. The cross-correlation is computed with fewer observations at larger lags and so it is common to truncate the function at a given level, which we do by only using the first T/4 lags, where T is the number of observations.&#x20;

A correlation below $$1.96/\sqrt{T}$$ is considered statistically insignificant, thus we optionally cut off the lags at this level (the modifier `sig-True` means we only use the statistically significant values, and `sig-False` means we use all values). We take the two summary statistics of the correlogram: the maximum over the considered lags (denoted by modifier `max`), and the average over the considered lags (`mean`).&#x20;

<details>

<summary>Cross Correlation Estimators</summary>

* `xcorr_max_sig-True`
* `xcorr_mean_sig-True`
* `xcorr_mean_sig-False`

</details>

<details>

<summary>Squared Cross Correlation Estimators</summary>

* `xcorr-sq_max_sig-True`
* `xcorr-sq_mean_sig-True`
* `xcorr-sq_mean_sig-False`

</details>

***

> ## **Kendall's Rank Correlation Coefficient**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, signed, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark><mark style="color:blue;">:</mark>_ `kendalltau`
>
> _<mark style="color:red;">Key Reference:</mark>_ [_\[1\]_](https://academic.oup.com/biomet/article-abstract/30/1-2/81/176907)

Kendall’s τ assesses the association of ordinal variables, similar to Spearman’s ρ, but has certain differences, such as becoming more mathematically tractable in the event of ties. The method is implemented via function [_kendalltau_](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kendalltau.html) in SciPy, and has a value in \[−1, 1].

<details>

<summary>Kendall's Rank Correlation Coefficient Estimator</summary>

* `kendalltau`

</details>

***

> ## Precision
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, linear, signed, multivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark><mark style="color:blue;">:</mark>_ `prec`

The precision matrix is the matrix inverse of the covariance matrix, and can be used to quantify the association between each pair of time series while controlling for concomitant effects of all other time series. For normalised time-series data, the precision matrix is equivalent to the partial correlation between each pairwise time series, conditioned on all other time series, and is within \[−1, 1]. The precision matrix is computed via the same module as the covariance matrix (in _scikit-learn_), and has the same estimators.

<details>

<summary>Precision Estimators</summary>

1. `prec_EmpiricalCovariance`
2. `prec_EllipticEnvelope`
3. `prec_MinCovDet`
4. `prec_GraphicalLasso`
5. `prec_GraphicalLassoCV`
6. `prec_ShrunkCovariance`
7. `prec_LedoitWolf`
8. `prec_OAS`

</details>

***

> ## Spearman's Rank-Correlation Coefficient
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, signed, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark><mark style="color:blue;">:</mark>_ `spearmanr`

Spearman’s ρ is a nonparametric measure of rank correlation between variables. The use of ordinal (ranked) variables allows the statistic to capture non-linear (but monotonic) relationships between random variables. The method is implemented via function [`spearmanr`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html) in [_SciPy_](https://github.com/scipy/scipy), and has a value in \[−1, 1].

<details>

<summary>Spearman's Rank-Correlation Coefficient Estimator</summary>

* `spearmanr`

</details>

***
