---
description: >-
  Spectral SPIs are computed in the frequency or time-frequency domain, using
  either Fourier or wavelet transformations to derive spectral matrices.
cover: >-
  https://images.unsplash.com/photo-1609669371960-f5f046c8e9e2?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxmcmVxdWVuY3l8ZW58MHx8fHwxNzAyNjAyMTg2fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Spectral

## Spectral SPIs Overview

Spectral SPIs are computed in the frequency or time-frequency domain, using either Fourier or wavelet transformations to derive spectral matrices. Unless otherwise stated, the frequency-domain (i.e., Fourier-based) measures are computed using the [_Spectral Connectivity Toolbox_](https://github.com/Eden-Kramer-Lab/spectral_connectivity). Each measure is based on the cross- and power-spectral densities at a given frequency and sampling rate, which is estimated via the multitaper technique. All directed measures (excluding parametric spectral Granger causality) are non-parametrically estimated through spectral matrix factorization. Moreover, the phase slope index is both computed in the frequency domain (via Fourier transformation) and the time-frequency domain (via Morlet transformation). For more detail on the calculation, limitations, or interpretation of each of the following measures, see the review by Bastos and Schoffelen.

Most methods in this literature category return a discrete set of values across a given frequency range. Specifically, there is one value per frequency bin, f, for a given sampling frequency, fs. In order to compare processes with very different timescales in this work (e.g., economic time series sampled daily, or neural activity sampled at millisecond scale), we consider the time step, ∆t (s), between successive measurements of a time series to be rescaled by a timescale, ts, appropriate for the process of interest, yielding a dimensionless time step, ∆t ̃ = ∆t/ts. Accordingly, we assume a sampling frequency fs = 1 throughout (denoted by modifier fs-1). We use 125 uniformly sampled bins across the entire frequency range, f ∈ \[f0, fs/2], where fs/2 is the Nyquist frequency and f0 = 4/T is chosen as the minimal frequency for computational reasons. In order to obtain SPIs, we take the mean (denoted by modifier mean) and the maximum (max) values of each measure over three ranges of the spectrum with corresponding modifiers, outlined below:

* `fmin-0_fmax-0-5:` the full-frequency range, f ∈ \[0, fs/2]
* `fmin-0_fmax-0-25:` the lower frequencies, f ∈ \[0, fs/4]
* `fmin-0-25_fmax-0-5:`the higher frequencies, f ∈ \[fs/4, fs/2].

***

> ## Coherence Magnitude
>
> _<mark style="color:blue;">Keywords: undirected, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `cohmag`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/17071228/)_,_ [_\[2\]_](https://pubmed.ncbi.nlm.nih.gov/26778976/)

The coherence (also known as coherence magnitude and ordinary coherence or coherence coefficient) is an undirected frequency-dependent measure of linear association between time series x and y. Mathematically, it is the frequency domain equivalent of the squared time-domain cross correlation. We compute mean (denoted by mean) and maximum (max) summary statistics of the coherence for the three frequency ranges.

<details>

<summary>Coherence Magnitude Estimators</summary>

* `cohmag_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `cohmag_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `cohmag_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `cohmag_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `cohmag_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `cohmag_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Coherence Phase
>
> _<mark style="color:blue;">Keywords: undirected, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier:</mark>_ `phase`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/26778976/)

By omitting the magnitude operator of the coherence, we obtain the complex-valued coherency, where the phase-difference angle (which we refer to as the coherence phase) has been used to infer a time-delayed dependence between two signals. We compute the coherence phase for the two summary statistics and three frequency ranges.

<details>

<summary>Coherence Phase Estimators</summary>

* `phase_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `phase_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `phase_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `phase_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `phase_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `phase_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Directed Coherence
>
> _<mark style="color:blue;">Keywords: directed, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `dcoh`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://www.lcs.poli.usp.br/~baccala/pdc/papers/asp.pdf)

The directed coherence is obtained from the inflow of x → y using the spectral transfer matrix, normalised by their noise covariance.

<details>

<summary>Directed Coherence Estimators</summary>

* `dcoh_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `dcoh_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `dcoh_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `dcoh_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `dcoh_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `dcoh_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Directed Transfer Function
>
> _<mark style="color:blue;">Keywords: directed, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `dtf`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://link.springer.com/article/10.1007/BF00198091)_,_ [_\[2\]_](https://www.sciencedirect.com/science/article/abs/pii/S0165027003000529)

The cross-spectral density matrix can be decomposed into a noise covariance matrix and a spectral transfer matrix, from which we obtain the directed transfer function (DTF), which quantifies the inflow from x → y (according to the transfer matrix) normalised by the total inflow from all other signals into y (the row-wise sum of the spectral transfer matrix). In addition to the DTF, we include direct-DTF (ddtf), which extends DTF by conditioning out the influence of other signals (using the partialized cross-spectrum).

<details>

<summary>Directed Transfer Function Estimators</summary>

* `dtf_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `dtf_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `dtf_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `dtf_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `dtf_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `dtf_multitaper_max_fs-1_fmin-0-25_fmax-0-5`
* `ddtf_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `ddtf_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `ddtf_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `ddtf_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `ddtf_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `ddtf_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Group Delay
>
> _<mark style="color:blue;">Keywords: directed, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `gd`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/6194969/)

The group delay infers a directed, average time-delay between two signals by measuring the slope of the phase differences (coherence phase) as a function of the frequency (obtained through linear regression). The slope is only computed for statistically significant coherence values and the time delay is obtained by a simple rescaling of the slope by 2π. We output the (rescaled) time-delay statistic for three frequency splits.

<details>

<summary>Group Delay Estimators</summary>

* `gd_multitaper_delay_fs-1_fmin-0_fmax-0-5`
* `gd_multitaper_delay_fs-1_fmin-0_fmax-0-25`
* `gd_multitaper_delay_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Imaginary Coherence
>
> _<mark style="color:blue;">Keywords: undirected, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `icoh`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/26778976/)_,_ [_\[2\]_](https://pubmed.ncbi.nlm.nih.gov/15351371/)

The imaginary part of the complex-valued coherency (referred to as imaginary coherence) is argued as a means to obtain the coherence exclusively caused by a time delay (i.e., by removing instantaneous interactions that are present in the real axis). We compute the imaginary coherence for the three typical frequency ranges and two summary statistics.

<details>

<summary>Imaginary Coherence Estimators</summary>

* `icoh_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `icoh_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `icoh_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `icoh_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `icoh_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `icoh_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Pairwise Phase Consistency
>
> _<mark style="color:blue;">Keywords: undirected, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `ppc`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/20114076/)

The pairwise phase consistency (PPC) measures phase synchronisation by quantifying the distribution of the (per taper) phase differences, which means that, unlike PLV, it is not biased by the sample size. We compute the PPC for the three typical frequency ranges and two summary statistics.

<details>

<summary>Pairwise Phase Consistency Estimators</summary>

* `ppc_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `ppc_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `ppc_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `ppc_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `ppc_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `ppc_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Partial Directed Coherence
>
> _<mark style="color:blue;">Keywords: directed, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `pdcoh`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/11417058/)_,_ [_\[2\]_](https://ieeexplore.ieee.org/document/4288544)

The partial directed coherence from x → y is the inflow (as per `DTF`) normalised by the total outflow from all other signals into y (the column-wise sum of the spectral transfer matrix). In addition to the partial directed coherence, we include the generalised partial directed coherence (`gpdoh`), which scales the relative strength of inflow from x → y by their noise covariance.

<details>

<summary>Partial Directed Coherence Estimators</summary>

* `pdcoh_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `pdcoh_multitaper_mean_fs-1_fmin-0_fmax-0-25`&#x20;
* `pdcoh_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`&#x20;
* `pdcoh_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `pdcoh_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `pdcoh_multitaper_max_fs-1_fmin-0-25_fmax-0-5`&#x20;
* `gpdcoh_multitaper_mean_fs-1_fmin-0_fmax-0-5`&#x20;
* `gpdcoh_multitaper_mean_fs-1_fmin-0_fmax-0-25`&#x20;
* `gpdcoh_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`&#x20;
* `gpdcoh_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `gpdcoh_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `gpdcoh_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Phase Lag Index
>
> _<mark style="color:blue;">Keywords: undirected, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifiers</mark>_: `pli`, `wpli`, `dspli`, `dswpli`
>
> _<mark style="color:red;">Key References:</mark>_ [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/17266107/)

The phase lag index (PLI) measures phase synchronisation by averaging the sign of the (per taper) phase difference. In addition to computing the phase lag index (`pli`) for the frequency domain, we include weighted variations that make the measure more robust against electrophysical artifacts (namely, field spread), noise, and sample-size bias by weighting components based on the imaginary coherence: the weighted phase lag index (`wpli`), the debiased squared phase lag index (`dspli`), and the debiased squared weighted phase lag index (`dswpli`).

<details>

<summary>Phase Lag Index Estimators</summary>

* `pli_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `pli_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `pli_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `pli_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `pli_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `pli_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

<details>

<summary>Weighted Phase Lax Index Estimators</summary>

* `wpli_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `wpli_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `wpli_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `wpli_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `wpli_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `wpli_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

<details>

<summary>Debiased Squared Phase Lag Index Estimators</summary>

* `dspli_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `dspli_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `dspli_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `dspli_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `dspli_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `dspli_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

<details>

<summary>Debiased Squared Weighted Phase Lag Index Estimators</summary>

* `dswpli_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `dswpli_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `dswpli_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `dswpli_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `dswpli_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `dswpli_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Phase Locking Value
>
> _<mark style="color:blue;">Keywords: undirected, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `plv`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://pubmed.ncbi.nlm.nih.gov/10619414/)

The phase locking value (PLV) is computed using the same formula as coherence, however applied to the amplitude-normalised Fourier transformed signals (i.e., normalised by individual tapers), which is argued to make it a more robust measure of phase synchronisation than the coherence measures presented earlier. We compute the PLV for the three typical frequency ranges and two summary statistics.

<details>

<summary>Phase Locking Value Estimators</summary>

* `plv_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `plv_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `plv_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `plv_multitaper_max_fs-1_fmin-0_fmax-0-5`
* `plv_multitaper_max_fs-1_fmin-0_fmax-0-25`
* `plv_multitaper_max_fs-1_fmin-0-25_fmax-0-5`

</details>

***

> ## Phase Slope Index
>
> _<mark style="color:blue;">Keywords: directed, linear/nonlinear, unsigned, bivariate, frequency-dependent and time-frequency dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `psi`
>
> _<mark style="color:red;">Key References</mark>_:[ _\[1\]_](https://pubmed.ncbi.nlm.nih.gov/18643502/)

The phase slope index (PSI) is a directed measure of information flow computed using the complex-valued coherency. Specifically, the phase slope index evaluates the consistency of the changes in phase differences across a pre-specified frequency range, weighted by the coherence. Due to its availability in v0.23.0 of the [_MEG and EEG Analysis python package_](https://github.com/mne-tools/mne-python) (MNE), this is the only spectral measure that is computed in the time-frequency domain (denoted by modifier `wavelet`) in addition to the frequency domain (denoted by `multitaper`). The time-frequency decomposition is given by a Morlet wavelet, with spectral densities indexed by a time-frequency tuple, given a sampling rate and number of cycles. To obtain a statistic in the time-frequency domain, we take the average (denoted by the mean modifier) or the maximum (max) PSI across all time points. We use the same pre-specified frequency ranges as outlined at the beginning of this section (i.e., low, high or full), giving three frequency-domain measures and six time-frequency domain measures due to the two summary statistics

<details>

<summary>Phase Slop Index Estimators</summary>

* `psi_multitaper_mean_fs-1_fmin-0_fmax-0-5`
* `psi_multitaper_mean_fs-1_fmin-0_fmax-0-25`
* `psi_multitaper_mean_fs-1_fmin-0-25_fmax-0-5`
* `psi_wavelet_mean_fs-1_fmin-0_fmax-0-5_mean`
* `psi_wavelet_mean_fs-1_fmin-0_fmax-0-25_mean`
* `psi_wavelet_mean_fs-1_fmin-0-25_fmax-0-5_mean`
* `psi_wavelet_max_fs-1_fmin-0_fmax-0-5_max`
* `psi_wavelet_max_fs-1_fmin-0_fmax-0-25_max`
* `psi_wavelet_max_fs-1_fmin-0-25_fmax-0-5_max`

</details>

***

> ## Spectral Granger Causality
>
> _<mark style="color:blue;">Keywords: directed, linear, unsigned, bivariate, frequency-dependent.</mark>_
>
> _<mark style="color:purple;">Base Identifier</mark>_: `sgc`
>
> _<mark style="color:red;">Key References</mark>_: [_\[1\]_](https://www.jstor.org/stable/1912791)_,_ [_\[2\]_](https://www.jstor.org/stable/2287238)_,_ [_\[3\]_](https://pubmed.ncbi.nlm.nih.gov/18394927/)_,_ [_\[4\]_](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.100.018701)

Spectral Granger causality is the frequency domain equivalent to Granger causality, computed via the spectral transfer matrix and noise covariance that are estimated using either a parametric (VAR model) approach or nonparametric (spectral factorisation) approach. We implement the nonparametric form from the [_Spectral Connectivity Toolbox_](https://github.com/Eden-Kramer-Lab/spectral_connectivity) and the parametric implementation from v0.9 of [_NiTime_](https://github.com/nipy/nitime/)_._ The VAR model is estimated via least squares using the same order for both processes, where the order is either inferred (denoted by modifier `order-None`) or fixed. We infer a VAR model order from the Bayesian information criterion (with a maximum lag of 50, implemented in the NiTime toolkit), as well as choosing fixed orders of one (denoted by `order-1`) and 20 (`order-20`). The combination of parametric and nonparametric approaches, with each autoregressive order and summary statistic.

<details>

<summary>Spectral Granger Causality Estimators</summary>

* `sgc_nonparametric_mean_fs-1_fmin-0_fmax-0-5`
* `sgc_nonparametric_mean_fs-1_fmin-0_fmax-0-25`
* `sgc_nonparametric_mean_fs-1_fmin-0-25_fmax-0-5`
* `sgc_nonparametric_max_fs-1_fmin-0_fmax-0-5`
* `sgc_nonparametric_max_fs-1_fmin-0_fmax-0-25`
* `sgc_nonparametric_max_fs-1_fmin-0-25_fmax-0-5`
* `sgc_parametric_mean_fs-1_fmin-0_fmax-0-5_order-None`
* `sgc_parametric_mean_fs-1_fmin-0_fmax-0-25_order-None`
* `sgc_parametric_mean_fs-1_fmin-0-25_fmax-0-5_order-None`
* `sgc_parametric_mean_fs-1_fmin-0_fmax-0-5_order-1`
* `sgc_parametric_mean_fs-1_fmin-0_fmax-0-25_order-1`
* `sgc_parametric_mean_fs-1_fmin-0-25_fmax-0-5_order-1`
* `sgc_parametric_mean_fs-1_fmin-0_fmax-0-5_order-20`
* `sgc_parametric_mean_fs-1_fmin-0_fmax-0-25_order-20`
* `sgc_parametric_mean_fs-1_fmin-0-25_fmax-0-5_order-20`
* `sgc_parametric_max_fs-1_fmin-0_fmax-0-5_order-None`
* `sgc_parametric_max_fs-1_fmin-0_fmax-0-25_order-None`
* `sgc_parametric_max_fs-1_fmin-0-25_fmax-0-5_order-None`
* `sgc_parametric_max_fs-1_fmin-0_fmax-0-5_order-1`
* `sgc_parametric_max_fs-1_fmin-0_fmax-0-25_order-1`
* `sgc_parametric_max_fs-1_fmin-0-25_fmax-0-5_order-1`
* `sgc_parametric_max_fs-1_fmin-0_fmax-0-5_order-20`
* `sgc_parametric_max_fs-1_fmin-0_fmax-0-25_order-20`
* `sgc_parametric_max_fs-1_fmin-0-25_fmax-0-5_order-20`

</details>

***
