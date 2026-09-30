---
cover: >-
  https://images.unsplash.com/photo-1561250056-32d1988f5157?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxkaWdpdHN8ZW58MHx8fHwxNzEzOTI1MzM1fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Information Theory

## Information Theoretic SPIs Overview

The pairwise measures that we employ from information theory are either intended to operate on serially independent observations (e.g., joint entropy and mutual information) or bivariate time series (e.g., transfer entropy and stochastic interaction). We primarily use _v1.6.1_ of the [_Java Information Dynamics Toolkit_](https://github.com/jlizier/jidt), which allows us to compute differential entropy, mutual information, and transfer entropy, in order to construct many information-theoretic measures. A density estimation is required to compute information-theoretic measures, and in this work we use **four** different estimators:

* The Gaussian-distribution model (denoted by modifier `gaussian`) assumes a linear-Gaussian multivariate, where the measure is derived from the cross-covariance matrix.
* Kernel estimation (kernel) uses a box kernel method with a specific kernel width (default width of 0.5 standard deviations (denoted by the modifier `W-0.5`).
* The Kozachenko–Leonenko technique (`kozachenko`) is a nearest-neighbour approach that is suitable when measures can only be constructed using entropy or joint entropy (not mutual information).
* The Kraskov–Stögbauer-Grassberger (KSG) technique (`ksg`) combines nearest-neighbour estimators for mutual information based measures (default of four nearest neighbours, indicated by modifier `NN-4`). The KSG estimator is effectively a combination of multiple Kozachenko estimators that includes techniques to remove a bias that is introduced by taking the difference between two differential entropy estimates.

An alternative approach to using the continuous estimators above would be to discretise each observation (e.g., through binning) and use a discrete estimator; however, discrete estimators are known to be heavily dependent on the discretisation size.

Density estimates for mutual information (and related measures) are biased by the autocorrelation present in individual signals. A common solution to reduce bias in nonlinear estimators (like the KSG technique) is to use dynamic correlation exclusion (also known as a Theiler window), which excludes any data points within a given time window from the density estimate and avoids oversampling. The window size should be large enough to render observations included in the density estimate uncorrelated (sometimes called the “autocorrelation time”); here, we set the window as the product of the autocorrelation functions of both time series (a heuristic for the autocorrelation time based on Bartlett’s formula). The `DCE` modifier indicates that the Theiler window is used for the KSG estimator.

***

> ## **Causally Conditioned Entropy**
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `cce`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://www.research-collection.ethz.ch/handle/20.500.11850/143796)_,_ [_\[2\]_](https://arxiv.org/abs/1002.1446)_,_ [_\[3\]_](https://citeseerx.ist.psu.edu/document?repid=rep1\&type=pdf\&doi=b7782af5fb454ed719f506462d38c7dc80e844a5)

Causally conditioned entropy aims to measure the uncertainty remaining in time series y in the context of the entire (causal) past of both time series x and y. The measure is computed as a sum of conditional entropies (of y given both the past of x and the past of y) with increasing history lengths. The standard assumption is that we consider the entire past of both time series (i.e., there are T−1 conditional entropies in the sum, from 1 to T−1); however, for computational reasons, we restrict the history length to 10. This corresponds to the assumption that the joint process is, at maximum, a 10th-order Markov chain.

<details>

<summary>Causally Conditioned Entropy Estimators</summary>

* `cce_gaussian`
* `cce_kozachenko`
* `cce_kernel_W-0.5`

</details>

***

> ## Conditional Entropy
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `ce`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://www.inference.org.uk/itprnn/book.pdf)

Conditional entropy quantifies the uncertainty over the observations in y in the context of simultaneously observing x.

<details>

<summary>Conditional Entropy Estimators</summary>

* `ce_gaussian`
* `ce_kozachenko`
* `ce_kernel_W-0.5`

</details>

***

> ## Directed Information
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `di`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://www.ce.cit.tum.de/fileadmin/w00cgn/lnt/staff/kramer/Papers/KramerThesis.pdf)_,_ [_\[2\]_](http://www.isiweb.ee.ethz.ch/papers/arch/mass-1990-1.pdf)_,_ [_\[3\]_](https://link.springer.com/book/10.1007/978-3-642-32952-4)

Directed information is a measure of information flow from a source time series x to a target time series y that is related to transfer entropy, but has no time lag between source and target. The directed information can be computed as a difference between the conditional entropy of y given its own past, and causally conditioned entropy. For the same reasons as for causally conditioned entropy, we restrict its computation up to a history length of 10.

<details>

<summary>Directed Information Estimators</summary>

* `di_gaussian`
* `di_kozachenko`
* `di_kernel_W-0.5`

</details>

***

> ## Granger Causality
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, linear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `gc`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://www.jstor.org/stable/1912791)

Granger causality is obtained by assessing directed dependence of x → y as the predictive power of a bivariate autoregressive model (comprising x and y) over the univariate autoregressive model (with y only). The statistic included in our framework is the log-ratio of residual variance for the two models, which is a variant of Granger causality popularised by Geweke later found to be equivalent to transfer entropy with a Gaussian estimator. We compute the time delay and embedding dimension in the same way as transfer entropy, allowing for both fixed and optimised embedding lengths (`k` and `l`) and time delays (`kt` and `lt`), where the optimisation procedure is identical to transfer entropy.

<details>

<summary>Granger Causality Estimators</summary>

* `gc_gaussian_k-max-10_tau-max-2`
* `gc_gaussian_k-1_kt-1_l-1_lt-1:`

</details>

***

> ## Integrated Information
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `phi`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://bmcneurosci.biomedcentral.com/articles/10.1186/1471-2202-5-42)_,_ [_\[2\]_](https://www.nature.com/articles/nrn.2016.44)_,_ [_\[3\]_](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004654)_,_ [_\[4\]_](https://www.researchgate.net/publication/3078630_On_information_rates_for_mismatched_decoders)_,_ [_\[5\]_](https://www.pnas.org/doi/full/10.1073/pnas.1603583113)

Integrated information was proposed to capture some aspects of consciousness as part of Integrated Information Theory (IIT). We implement two proxy measures for _IIT 2.0_ from the [PhiToolbox](https://github.com/oizumi-lab/PhiToolbox):

* $$\phi^*$$ is a proxy of integrated information. It is an undirected measure that uses the concept of mismatched decoding in information theory and can be considered as the amount of loss of information due to the disconnection between two variables.
* $$\phi_G$$ is a measure of integrated information derived from information geometry. It is an undirected measure that quantifies the divergence between the actual probability distribution of a system and an approximate probability distribution where influences among elements are statistically disconnected. Here we implement the Gaussian-distribution model.

Both measures are optionally normalised (divided) by entropy (denoted by the modifier `norm`).

<details>

<summary>Integrated Information Estimators</summary>

* `phi_star_t-1_norm-0`
* `phi_star_t-1_norm-1`
* `phi_Geo_t-1_norm-0`
* `phi_Geo_t-1_norm-1`

</details>

***

> ## Joint Entropy
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `je`
>
> _<mark style="color:red;">Key References</mark>:_ [_\[1\]_](https://www.inference.org.uk/itprnn/book.pdf)

The joint entropy quantifies the uncertainty over the paired observations.

<details>

<summary>Joint Entropy Estimators</summary>

* `je_gaussian`
* `je_kozachenko`
* `je_kernel_W-0.5`

</details>

***

> ## Mutual Information
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, contemporaneous.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `mi`
>
> _<mark style="color:red;">Key References</mark>:_ [_\[1\]_](https://www.inference.org.uk/itprnn/book.pdf)

Mutual information is an undirected measure of the (potentially nonlinear) dependence between paired observations of x and y.

<details>

<summary>Mutual Information Estimators</summary>

* `mi_gaussian`
* `mi_kraskov_NN-4`
* `mi_kraskov_NN-4_DCE`
* `mi_kernel_W-0.25`

</details>

***

> ## Stochastic Interaction
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: undirected, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `si`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://www.mdpi.com/1099-4300/17/4/2432)_,_ [_\[2\]_](https://www.sciencedirect.com/science/article/abs/pii/S0925231202007324)

Stochastic interaction is a measure of integrated information between two processes in the context of their own past. It is quantified by the difference between the joint entropy of the bivariate process and the individual entropies of each univariate process. Both entropies are measured in context of (conditioned on) the history of the processes; we restrict this history to be only one-step, assuming a first-order Markov process.

<details>

<summary>Stochastic Interaction Estimators</summary>

* `si_gaussian_k-1`
* `si_kozachenko_k-1`
* `si_kernel_W-0.5_k-1`

</details>

***

> ## Transfer Entropy
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `te`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.85.461)

Transfer entropy is a measure of information transfer from a source time series `x` to a target time series `y`, based on the Takens time-delay embedding. Delay embeddings capture the relevant history of a time series that can be used as a predictor of its future and are constructed from an embedding length and a time delay. The embedding lengths are denoted by modifier’s `l` and `k` for the source, `x`, and target, `y`, respectively; the time delay is denoted `lt` and `kt` for time series `x` and `y`. The embedding parameters can be obtained in a number of ways, so long as their product is (significantly) less than the number of observations. We compute transfer entropy for both fixed and optimised embedding parameters. The fixed parameters are minimal values, i.e., typically with a fixed target embedding length of 1 (denoted by `k-1`) or 2 (denoted by `k-2`).

The optimised parameters are inferred by choosing the embedding parameters (up to a maximum embedding length of 10, denoted by `k-max-10`, and maximum time delay of 2, `tau-max-2`) that maximise a univariate information-theoretic measure known as active information storage. Note that there is no inclusion of the Gaussian estimator for transfer entropy, since this is equivalent to Granger causality. We also include a symbolic estimator for transfer entropy (denoted by the modifier `symbolic`).

<details>

<summary>Transfer Entropy Estimators</summary>

* `te_kraskov_NN-4_k-max-10_tau-max-4`
* `te_kraskov_NN-4_DCE_k-max-10_tau-max-4`&#x20;
* `te_kraskov_NN-4_DCE_k-1_kt-1_l-1_lt-1`
* `te_kraskov_NN-4_DCE_k-2_kt-1_l-1_lt-1`
* `te_kraskov_NN-4_k-1_kt-1_l-1_lt-1`
* `te_kernel_W-0.25_k-1`&#x20;
* `te_symbolic_k-1_kt-1_l-1_lt-1`
* `te_symbolic_k-10_kt-1_l-1_lt-1`

</details>

***

> ## Time-lagged Mutual Information
>
> [_<mark style="color:blue;">Keywords</mark>_](../glossary-of-terms.md#keywords)_<mark style="color:blue;">: directed, nonlinear, unsigned, bivariate, time-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `tlmi`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.85.461)

Time-lagged mutual information is a directed measure of the dependence between time series `x` and a time-lagged instance of the series `y`. We include statistics for only lag-one mutual information, giving the following estimators:

<details>

<summary>Time-lagged MI Estimators</summary>

* `tlmi_gaussian`
* `tlmi_kraskov_NN-4`
* `tlmi_kraskov_NN-4_DCE`
* `tlmi_kernel_W-0.25`

</details>

***
