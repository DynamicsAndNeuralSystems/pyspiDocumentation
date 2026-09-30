---
cover: >-
  https://images.unsplash.com/photo-1545987796-200677ee1011?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxjb25uZWN0aW9uc3xlbnwwfHx8fDE3MTM5MjUyMTZ8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Causal Inference

## Causal Inference SPIs Overview

These statistics aim to establish directed independence from bivariate observations, typically by making assumptions about the underlying model. We use two packages:

* For convergent cross-mapping, we use the [_Empirical Dynamic Modeling_](https://github.com/SugiharaLab/pyEDM) (pyEDM) package.
* For all other SPIs, we use _v0.5.23_ of the [_Causal Discovery Toolbox_](https://github.com/FenTechSolutions/CausalDiscoveryToolbox) (cdt).

***

> ## Additive Noise Model
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark><mark style="color:blue;">:</mark>_ `anm`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://papers.nips.cc/paper_files/paper/2008/file/f7664060cc52bc6f3d620bcedc94a4b6-Paper.pdf)

Additive noise models are used for hypothesis testing directed nonlinear dependence (or causality) of x → y by making the assumption that the effect variable, y, is a function of a cause variable, x, plus a noise term (that is independent of the cause). In this framework we use the statistic from cdt as our SPI, which is computed by first predicting y from x via a Gaussian process (with a radial basis function kernel), and then computing the normalized HSIC test statistic from the residuals.

<details>

<summary>Additive Noise Estimator</summary>

* `anm (`<mark style="color:blue;">**`ANM`**</mark>`)`

</details>

***

> ## **Conditional Distribution Similarity Fit**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark><mark style="color:blue;">:</mark>_ `cds`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://arxiv.org/abs/1601.06680)

The conditional distribution similarity fit is the standard deviation of the conditional probability distribution of y given x, where the distributions are estimated by discretizing the values.

<details>

<summary>Conditional Distribution Similarity Fit Estimator</summary>

* `cds`

</details>

***

> ## **Convergent Cross-Mapping**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `ccm`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://www.science.org/doi/10.1126/science.1227079)

The idea behind convergent cross-mapping (CCM) is that there is a causal influence from time series x → y if the Takens time-delay embedding of y can be used to predict the observations of x. The algorithm quantifies the prediction error (in terms of Pearson’s ρ) of time series x from the delay embedding of time series y for increasing library sizes (i.e., time series length being used in the predictions). If, as the library size increases, the correlation converges and is higher in one direction than the other, there is an inferred causal link. The results of CCM are typically represented as two curves; one for each causal direction (x → y and y → x) with the library size on the horizontal axis and the prediction quality (correlation) on the vertical axis. We use the [_pyEDM_ package](https://github.com/SugiharaLab/pyEDM) to compute CCM, which requires an embedding dimension to be set for the delay embedding of each time series.

We use both fixed embedding dimensions (with dimension 1 and 10, indicated by modifiers `E-1` and `E-10`, respectively) and an inferred embedding dimension from univariate phase-space reconstruction methods (modifier `E-None`). Following the Supplementary Materials of the original CCM paper and the documentation in the pyEDM package, we infer the embedding dimension to be the maximum of the two univariate delay embeddings that best predicted each time series. Given a fixed or inferred embedding dimension, we have an upper and lower bound on the minimum and maximum library size that can be used for computing CCM. In this work we use 21 uniformly sampled library sizes between this minimum and maximum to generate the CCM curves. Once the curve (prediction quality as a function of library size) is obtained, we take summary statistics of the mean (modifier `mean`), maximum (`max`) and difference (`diff`) across the curves. We do not explicitly measure convergence of the algorithm as a function of library size, consistent with common practice in the literature, but note that this differs from the original theory; no automatic algorithm or heuristic was originally proposed for quantifying convergence.&#x20;

<details>

<summary>Convergent Cross-Mapping Estimators</summary>

* `ccm_E-1_mean`
* `ccm_E-1_max`
* `ccm_E-1_diff`
* `ccm_E-10_mean`
* `ccm_E-10_max`
* `ccm_E-10_diff`
* `ccm_E-None_mean`
* `ccm_E-None_max`
* `ccm_E-None_diff`

</details>

***

> ## **Information-Geometric Conditional Independence**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `igci`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://arxiv.org/abs/1203.3475)

Information-geometric conditional independence is a method for inferring causal influence from x → y for deterministic systems with invertible functions. The statistic is computed using _cdt_ as the difference in differential entropies where the probability density is computed via nearest-neighbor estimators.

<details>

<summary>Information-Geometric Causal Inference Estimator</summary>

* `igci`

</details>

***

> ## **Regression Error-Based Causal Inference**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `reci`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://proceedings.mlr.press/v84/bloebaum18a/bloebaum18a.pdf)

The regression error-based causal inference method is an estimate of the causal effect of x → y by quantifying the error in a regression of y on x with a monomial (power product) model. In the bivariate case, this statistic is the MSE of the linear regression of the cubic (plus constant) of x with y.

<details>

<summary>Regression Error-Based Causal Inference Estimator</summary>

* `reci`

</details>

***
