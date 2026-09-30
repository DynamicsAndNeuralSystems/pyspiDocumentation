---
coverY: 0
---

# Miscellaneous

## Miscellaneous SPIs Overview

A small number of methods do not fit squarely into any category listed above, and so we place them in a ‘miscellaneous’ category. Here, we outline the use of linear and nonlinear model fits, for which we use [_scikit-learn_](https://github.com/scikit-learn/scikit-learn)_,_ cointegration, for which we use [_statsmodels_](https://github.com/statsmodels/statsmodels), and envelope correlation, for which we use [_MNE_](https://github.com/mne-tools/mne-python).

***

> ## Cointegration
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, linear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `coint`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://www.jstor.org/stable/1913236)_,_ [_\[2\]_](https://www.cambridge.org/core/books/abs/econometrics-and-economic-theory-in-the-20th-century/an-autoregressive-distributedlag-modelling-approach-to-cointegration-analysis/0A3624D5C624BED6E2A963190403653A)

If two time series are individually integrated but some linear combination of them has a lower order of integration, then the series are said to be ‘cointegrated’. We implement statistics for quantifying the cointegration of bivariate time series from two tests included in _v0.12.0_ of [_statsmodels_](https://github.com/statsmodels/statsmodels):&#x20;

* **Augmented Engle-Granger** (AEG, modifier `aeg`) **two-step test.**  For the AEG test, we use a lag that is inferred via either AIC (modifier `aic`) or BIC (`bic`), with a maximum lag of 10 and obtain a t-statistic (`tstat`) of the unit-root test on the residuals. The time series is first detrended by assuming either a constant (`c`) or a constant and linear trend (`ct`).
* **Johansen test** (`johansen`). For the Johansen test, we output both the maximum eigenvalue (modifier `max_eig_stat`) and the trace (`trace_stat`) of the vector error correction model. Similar to the AEG test, we also assume a constant (`order-0`) or constant and linear trend (`order-1`) and fixed autoregressive lags of 1 (`ardiff-1`) and 10 (`ardiff-10`).

<details>

<summary>Cointegration Estimators</summary>

* `coint_johansen_max_eig_stat_order-0_ardiff-10`
* `coint_johansen_trace_stat_order-0_ardiff-10`
* `coint_johansen_max_eig_stat_order-0_ardiff-1`
* `coint_johansen_trace_stat_order-0_ardiff-1`
* `coint_johansen_max_eig_stat_order-1_ardiff-10`
* `coint_johansen_trace_stat_order-1_ardiff-10`
* `coint_johansen_max_eig_stat_order-1_ardiff-1`&#x20;
* `coint_johansen_trace_stat_order-1_ardiff-1`
* `coint_aeg_tstat_trend-c_autolag-aic_maxlag-10`
* `coint_aeg_tstat_trend-ct_autolag-aic_maxlag-10 (`<mark style="color:blue;">**`cointegration`**</mark>`)`
* `coint_aeg_tstat_trend-ct_autolag-bic_maxlag-10`

</details>

***

> ## Gaussian Process Model Fit
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `gpfit`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://mlg.eng.cam.ac.uk/pub/pdf/WilRas96.pdf)

Similar to the linear model fits, we also use Gaussian process model fits as a nonparametric measure of influence of `x` on `y`. Here, we use a combination of kernels (with parameters chosen from default settings) from the scikit-learn package and compute the MSE of their fit. The fits are computed for the dot-product kernel with inhomogenity parameter σ\_0 = 1 (`DotProduct`) and the radial basis function (RBF) kernel with length scale l = 1 (`RBF`). Each of these kernels are separately combined with the constant kernel (with a constant of 1.0) and the white kernel (with a noise level of 1).

<details>

<summary>Gaussian Process Model Fit</summary>

* `gpfit_DotProduct`
* `gpfit_RBF`

</details>

***



> ## Linear Model Fit
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, linear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `lmfit`

Linear regression is commonly used for establishing independence through model fits (e.g., see additive noise models). As such, we use a number of linear models and record the mean squared error (MSE) of a regression of `y` on `x`.

The following models (with the default parameters) from scikit-learn are included: stochastic gradient descent regression with a squared loss (ordinary least squares fit) function (denoted by modifier `SGDRegressor`); Ridge regression, which uses l2-norm regularization (`Ride`); the Elastic-Net model, which uses both l1 and l2-norm regularization (`ElasticNet`); and the Bayesian Ridge regressor uses a gamma distribution prior (with λ1 = λ2 = 10^(-6)}) for the l2-norm regularizor in Ridge regression (`BayesianRidge`).

<details>

<summary>Linear Model Fit Estimators</summary>

* `lmfit_SGDRegressor`
* `lmfit_Ridge`
* `lmfit_ElasticNet`
* `lmfit_BayesianRidge`

</details>

***

> ## Power Envelope Correlation
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, linear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `pec`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/22561454/)_,_ [_\[2\]_](https://pubmed.ncbi.nlm.nih.gov/29462724/)

The envelope correlation is the correlation between the two amplitude envelopes of `x` and `y`. Power envelope correlation is computed using MNE, where we use six combinations of parameters, including whether the method was orthogonalised (modifier `orth`), the envelopes were squared and logs were taken prior to correlation (`log`), or the absolute of the correlation coefficient was used (`abs`).

<details>

<summary>Power Envelope Correlation Estimators</summary>

* `pec`
* `pec_orth`
* `pec_log`
* `pec_orth_log`
* `pec_orth_abs`
* `pec_orth_log_abs`

</details>

***

> ## Interdependence Score
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">:</mark>_ <mark style="color:blue;">undirected, nonlinear, unsigned</mark>
>
> _<mark style="color:purple;">Base Identifier</mark>_: `ids`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://www.pnas.org/doi/epdf/10.1073/pnas.2509860122)

The InterDependenceScore (IDS) is a fast and scalable measure of dependence that captures both linear and various non-linear dependencies among random variables. Based on the Hilbert-Schmidt Independence Criterion (HSIC), the IDS offers a finite-dimensional approximation of the universal kernels that can detect any type of dependence, allowing the method to scale efficeintly to large datasets.&#x20;

Unlike HSIC, the IDS is normalized to a range of \[0, 1] with a score of 0 indicating independence, and larger values corresponding to a greater degree of dependence. This implementation of the IDS is based on the code made publicly available [here](https://github.com/aradha/interdependence_scores/tree/main).&#x20;

<details>

<summary>Interdependence Score Estimators</summary>

* `ids`

</details>
