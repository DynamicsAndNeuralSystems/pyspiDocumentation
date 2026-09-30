---
description: A tutorial by Annie G. Bryant from the MDIG 2026 Summer School Workshop.
cover: ../../../.gitbook/assets/Screenshot 2026-07-16 at 12.50.06 pm.png
coverY: 0
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

# Motion tracking

{% hint style="info" %}
This tutorial was adapted from the Jupyter notebooks available in the original repository. For an interactive tutorial, [access the original repository here](https://github.com/anniegbryant/MDIG2026_pyspi_tutorial/tree/main). You can also access the accompanying slides from the workshop [here](https://github.com/anniegbryant/MDIG2026_pyspi_tutorial/blob/main/MDIG2026_pyspi_AGB.pdf).&#x20;
{% endhint %}

**Download the slides from the MDIG 2026 Summer School Workshop:**

{% file src="../../../.gitbook/assets/MDIG2026_pyspi_AGB.pdf" %}

#### Author: Annie G. Bryant, PhD  <a href="#author-annie-g.-bryant-phd" id="author-annie-g.-bryant-phd"></a>

A hands-on tour of [`pyspi`](https://github.com/DynamicsAndNeuralSystems/pyspi) (Python-based Statistics for Pairwise Interactions), applied to real motion-tracking data from a dyadic interaction task. Starting from a single pair of time series and a single statistic, we build up through five levels of analysis:

1. **Level 1** — one pair of time series (hip sway), one statistic (covariance), computed by hand
2. **Level 2** — the same pair, now compared with the `sonnet` SPI subset (14 statistics)
3. **Level 3** — the same pair, compared with the `fast` SPI subset
4. **Level 4** — the `sonnet` subset applied across four distinct body-part pairs
5. **Level 5** — a 4-node multivariate system (both hips and both noses), analysed jointly

We close with a short extension to real resting-state fMRI data, to show the same pipeline scales from motion capture to brain imaging.

## Setup

Let's start by **(1)** installing the required packages for this tutorial, and **(2)** importing them:

```bash
!pip install pyspi pandas matplotlib rpy2 oct2py
```

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import colors
import scipy
from copy import deepcopy

# pyspi-specific imports 
from pyspi.calculator import Calculator
from pyspi.data import Data
from pyspi.statistics.basic import Covariance, CrossCorrelation
from pyspi.statistics.infotheory import DirectedInfo

# Add rpy2 to run R code chunks
%load_ext rpy2.ipython
```

We'll also bring in R via `rpy2`, so that later in the notebook we can hand off long-format results to `ggplot2`/`cowplot` for plotting. The cell below loads those R packages and sets `theme_cowplot()` as the default `ggplot2` theme.

```r
%%R 

install.packages(c("cowplot", "tidyverse", "ggseg", "patchwork", "sf"), repos = "http://cran.us.r-project.org")

suppressPackageStartupMessages({
    library(cowplot)
    library(ggseg)
    library(ggseg.formats)
    library(patchwork)
    library(tidyverse)
})

# Set cowplot theme
theme_set(theme_cowplot())
```

## :arrow\_down: Loading the example data

Now we can load in and view the example data for this tutorial: motion-tracking time series from a single trial of a dyadic guessing game, in which one participant (the clue-giver) silently acts out a word for the other (the guesser) to identify. Body landmark coordinates were tracked separately for each participant, as described in [Li et al. _JASA_ (2026)](https://doi.org/10.1121/10.0043950).

Specifically, in this set of exercises, we will work with Participant 103 as the clue-giver and Participant 203 as the guesser, with the word 'balloon' as the target word in the Taboo game.

```python
# Time series example datasets: Body tracking data of clue-giver versus guesser with the word 'balloon'
clue_giver_body_TS = pd.read_csv('https://github.com/WimPouw/TilburgMultiscaleSummerschool2026/raw/refs/heads/main/Datasets/BalanceCorpus/motiontracking/Output_TimeSeries/103_203_14_1_20250113_152536_balloon_board_clueGiver_cam01_body.csv')
guesser_body_TS = pd.read_csv('https://github.com/WimPouw/TilburgMultiscaleSummerschool2026/raw/refs/heads/main/Datasets/BalanceCorpus/motiontracking/Output_TimeSeries/103_203_14_1_20250113_152536_balloon_board_guesser_cam01_body.csv')

# Print size of the datasets
print(f"Clue-giver body time series shape: {clue_giver_body_TS.shape}")
print(f"Guesser body time series shape: {guesser_body_TS.shape}")

# Print first five rows of clue-giver body time series
clue_giver_body_TS.head()
```

**Expected output:**

```
Clue-giver body time series shape: (663, 133)
Guesser body time series shape: (663, 133)
```

| time | X\_NOSE    | Y\_NOSE  | Z\_NOSE  | visibility\_NOSE | X\_LEFT\_EYE\_INNER | Y\_LEFT\_EYE\_INNER | Z\_LEFT\_EYE\_INNER | visibility\_LEFT\_EYE\_INNER | X\_LEFT\_EYE | ...      | Z\_RIGHT\_HEEL | visibility\_RIGHT\_HEEL | X\_LEFT\_FOOT\_INDEX | Y\_LEFT\_FOOT\_INDEX | Z\_LEFT\_FOOT\_INDEX | visibility\_LEFT\_FOOT\_INDEX | X\_RIGHT\_FOOT\_INDEX | Y\_RIGHT\_FOOT\_INDEX | Z\_RIGHT\_FOOT\_INDEX | visibility\_RIGHT\_FOOT\_INDEX |          |
| ---- | ---------- | -------- | -------- | ---------------- | ------------------- | ------------------- | ------------------- | ---------------------------- | ------------ | -------- | -------------- | ----------------------- | -------------------- | -------------------- | -------------------- | ----------------------------- | --------------------- | --------------------- | --------------------- | ------------------------------ | -------- |
| 0    | 0.000000   | 0.569139 | 0.225114 | -0.316071        | 0.999998            | 0.574026            | 0.207569            | -0.301978                    | 0.999993     | 0.577072 | ...            | 0.163274                | 0.876503             | 0.604174             | 0.898399             | 0.161986                      | 0.991762              | 0.551458              | 0.908794              | 0.046576                       | 0.989293 |
| 1    | 33.333333  | 0.569288 | 0.225822 | -0.315999        | 0.999998            | 0.574147            | 0.208270            | -0.301998                    | 0.999993     | 0.577160 | ...            | 0.195436                | 0.877414             | 0.601905             | 0.899552             | 0.149456                      | 0.991485              | 0.546144              | 0.908693              | 0.086486                       | 0.989041 |
| 2    | 66.666667  | 0.569262 | 0.226569 | -0.314877        | 0.999998            | 0.574114            | 0.209145            | -0.301370                    | 0.999993     | 0.577128 | ...            | 0.194734                | 0.877757             | 0.600369             | 0.900433             | 0.137130                      | 0.991186              | 0.543857              | 0.908689              | 0.085700                       | 0.988748 |
| 3    | 100.000000 | 0.569043 | 0.226605 | -0.312801        | 0.999998            | 0.573951            | 0.209254            | -0.299398                    | 0.999993     | 0.576985 | ...            | 0.194149                | 0.877923             | 0.600063             | 0.901379             | 0.136920                      | 0.990887              | 0.543013              | 0.908684              | 0.083756                       | 0.988497 |
| 4    | 133.333333 | 0.568809 | 0.226604 | -0.305484        | 0.999998            | 0.573763            | 0.209267            | -0.292139                    | 0.999992     | 0.576812 | ...            | 0.200337                | 0.877835             | 0.599682             | 0.901814             | 0.132546                      | 0.990600              | 0.542135              | 0.908652              | 0.091640                       | 0.988199 |

***

## :beginner:Level 1: Hip time series for clue-giver versus guesser

We'll begin with the simplest possible case: a single pair of time series, compared with a single statistic. The signal of interest is _hip sway_, the average x-coordinate of the left and right hip landmarks, compared between the clue-giver and the guesser.

```python
clue_giver_hip_TS = (clue_giver_body_TS["X_LEFT_HIP"] + clue_giver_body_TS["X_RIGHT_HIP"]) / 2
guesser_hip_TS = (guesser_body_TS["X_LEFT_HIP"] + guesser_body_TS["X_RIGHT_HIP"]) / 2

# Manually apply linear detrending and z-scoring to the time series, 
# only for visualization purposes (this is applied separately in pyspi)
clue_giver_hip_TS_detrend = scipy.stats.zscore(scipy.signal.detrend(clue_giver_hip_TS))
guesser_hip_TS_detrend = scipy.stats.zscore(scipy.signal.detrend(guesser_hip_TS))

# Combine the two time series into a two-column array
both_hip_TS = pd.DataFrame({
    'Clue-giver': clue_giver_hip_TS_detrend,
    'Guesser': guesser_hip_TS_detrend,
    'Time': clue_giver_body_TS['time']
})

both_hip_TS.head()
```

**Output**:

| index | Clue-giver | Guesser   | Time       |
| ----- | ---------- | --------- | ---------- |
| 0     | 0.422433   | -2.556808 | 0.000000   |
| 1     | 0.250746   | -2.459046 | 33.333333  |
| 2     | 0.189964   | -2.382114 | 66.666667  |
| 3     | 0.178308   | -2.316100 | 100.000000 |
| 4     | 0.171072   | -2.166736 | 133.333333 |

Plotting the two detrended, z-scored series together shows how closely the clue-giver's and guesser's hip sway track each other over the course of the trial.

```python
# Plot the time series
plt.figure()
both_hip_TS.plot(x='Time', y=['Clue-giver', 'Guesser'])

ax = plt.gca()
ax.legend(labels=['Clue-giver', 'Guesser'], loc='center left', bbox_to_anchor=(1, 0.5))
plt.xlabel('Time (ms)')
plt.ylabel('Hip x-coordinate (average)')
plt.show()
```

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

A scatterplot of the same two series gives a complementary view: the tighter and more linear the cloud of points, the stronger the (linear) association between the two signals.

```python
# Scatter plot of the two time series
plt.figure()
plt.scatter(both_hip_TS['Clue-giver'], both_hip_TS['Guesser'], alpha=0.5)
plt.xlabel('Clue-giver Hip x-coordinate (z-score)')
plt.ylabel('Guesser Hip x-coordinate (z-score)')
plt.title('Clue-giver vs. Guesser Hip x-coordinates')
plt.show()
```

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Now let's quantify that association numerically, starting with the simplest possible statistic: the Pearson correlation, equivalent to `pyspi`'s `Covariance` SPI applied to two detrended, z-scored series.

```python
# Compute covariance between the two time series
data = Data(np.stack([clue_giver_hip_TS, guesser_hip_TS]), detrend=True, normalise=True)
spi = Covariance()
cov_matrix = spi.multivariate(data)  # 2x2, diagonal is NaN by design
val = cov_matrix[0, 1]

print(f"Pearson correlation between Clue-giver and Guesser hip x-coordinates: {val}")
```

**Output**:

```
[1/2] Detrending time series in the dataset...
[2/2] Normalising (z-scoring) each time series in the dataset...

Pearson correlation between Clue-giver and Guesser hip x-coordinates: -0.29082533267389915
```

***

## Level 2: sonnet subset, same time series pair

Level 2 keeps the same clue-giver/guesser hip pair, but replaces the single covariance statistic with `pyspi`'s `sonnet` subset — a curated set of 14 SPIs spanning basic, distance, causal, information-theoretic, spectral, wavelet, and miscellaneous statistic families. This gives a much richer picture of how the two time series relate to one another.

We compute the full [`Calculator`](../../../information-about-pyspi/api-reference/pyspi.calculator.calculator.md) on the same pair, then reshape the wide results table into long format (one row per SPI per direction) so it's easy to filter and plot:

```python
# Sonnet subset
calc_level2 = Calculator(dataset=data, subset='sonnet')
calc_level2.compute()

# Convert statistics results to a readable table
table_level2 = calc_level2.table
table_level2.columns = table_level2.columns.to_flat_index()

table_level2 = table_level2.rename(columns='__'.join).assign(role_from = lambda x: x.index)
table_level2_long = table_level2.melt(id_vars='role_from', var_name='SPI__role_to', value_name='value')

# Reshape from wide to long, and add columns for person's role
table_level2_long["SPI"] = table_level2_long["SPI__role_to"].str.split("__").str[0]
table_level2_long["role_to"] = table_level2_long["SPI__role_to"].str.split("__").str[1]

table_level2_long = (table_level2_long.assign(role_from = lambda x: np.where(x.role_from == 'proc-0', 'Clue-giver', 'Guesser'),
                                    role_to = lambda x: np.where(x.role_to == 'proc-0', 'Clue-giver', 'Guesser'))
                    .drop(columns=['SPI__role_to'])
                    .query("role_from != role_to")
                    )

table_level2_long.head()
```

**Output:**

| index | role\_from | value     | SPI                      | role\_to   |
| ----- | ---------- | --------- | ------------------------ | ---------- |
| 1     | Guesser    | -0.290825 | cov\_EmpiricalCovariance | Clue-giver |
| 2     | Clue-giver | -0.290825 | cov\_EmpiricalCovariance | Guesser    |
| 5     | Guesser    | 32.626012 | dtw\_constraint-itakura  | Clue-giver |
| 6     | Clue-giver | 32.626012 | dtw\_constraint-itakura  | Guesser    |
| 9     | Guesser    | 0.112570  | bary\_dtw\_mean          | Clue-giver |

Passing the long-format table into R lets us visualize all 14 SPI values side-by-side, faceted by statistic:

```r
%%R -i table_level2_long -w 800 -h 700

library(ggplot2)
library(tidyverse)

table_level2_long %>%
    mutate(SPI = fct_reorder(SPI, value, .fun = median)) %>% 
    ggplot(data=., mapping=aes(x=role_from, y=value, fill=role_from)) +
    geom_bar(stat="identity", position=position_dodge()) +
    coord_flip() +
    geom_hline(yintercept=0, linetype="dashed", color = "black") +
    facet_wrap(~SPI, scales='free', strip.position = 'top', ncol=2) +
    ylab('SPI value') +
    labs(fill='Role of source person') +
    theme(axis.text.y = element_blank(),
          axis.ticks.y = element_blank(),
          axis.title.y = element_blank(),
          strip.background = element_blank(),
          legend.position = 'top')
```

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

***

## Level 3: Fast SPI subset, same pair of time series (hip x-position in clue-giver vs. guesser) <a href="#level-3-fast-spi-subset-same-pair-of-time-series-hip-x-position-in-clue-giver-vs.-guesser" id="level-3-fast-spi-subset-same-pair-of-time-series-hip-x-position-in-clue-giver-vs.-guesser"></a>

Level 3 repeats the same hip-sway pair once more, this time with the `fast` SPI subset — a smaller set of computationally cheap statistics, useful when scaling up to many time-series pairs or nodes.

```python
# Fast subset
calc_level3 = Calculator(dataset=data, subset='fast')
calc_level3.compute()

# Convert statistics results to a readable table
table_level3 = calc_level3.table
table_level3.columns = table_level3.columns.to_flat_index()

table_level3 = table_level3.rename(columns='__'.join).assign(role_from = lambda x: x.index)
table_level3_long = table_level3.melt(id_vars='role_from', var_name='SPI__role_to', value_name='value')

# Reshape from wide to long, and add columns for person's role
table_level3_long["SPI"] = table_level3_long["SPI__role_to"].str.split("__").str[0]
table_level3_long["role_to"] = table_level3_long["SPI__role_to"].str.split("__").str[1]

table_level3_long = (table_level3_long
                     .assign(role_from = lambda x: np.where(x.role_from == 'proc-0', 'Clue-giver', 'Guesser'),
                             role_to = lambda x: np.where(x.role_to == 'proc-0', 'Clue-giver', 'Guesser'))
                    .drop(columns=['SPI__role_to'])
                    .query("role_from != role_to")
                    )
table_level3_long.head()
```

**Output:**

| index | role\_from | value     | SPI                      | role\_to   |
| ----- | ---------- | --------- | ------------------------ | ---------- |
| 1     | Guesser    | -0.290825 | cov\_EmpiricalCovariance | Clue-giver |
| 2     | Clue-giver | -0.290825 | cov\_EmpiricalCovariance | Guesser    |
| 5     | Guesser    | -0.280825 | cov\_GraphicalLasso      | Clue-giver |
| 6     | Clue-giver | -0.280825 | cov\_GraphicalLasso      | Guesser    |
| 9     | Guesser    | -0.000000 | cov\_GraphicalLassoCV    | Clue-giver |

***

## Level 4: 'Sonnet' subset across four distinct time-series pairs

Having looked at a single pair with two different SPI subsets, Level 4 keeps the `sonnet` subset fixed and instead expands the number of time-series pairs — moving from hips alone to four distinct body-part comparisons.

In addition to the average left/right x-position (hip sway), we will now expand our scope to an additional three time-series examples from the same dataset:

1. `Y_RIGHT_WRIST`: y-coordinates of the right wrist in the clue-giver versus guesser, capturing gesture kinematics
2. `Y_NOSE`: y-coordinates of the nose in the clue-giver versus guesser, capturing head movement/nodding
3. `X_RIGHT_SHOULDER` + `X_LEFT_SHOULDER`: as with the hips, the average x-coordinate between left/right shoulders in clue-giver versus guesser

```python
# Y_RIGHT_WRIST
clue_giver_right_wrist_TS = clue_giver_body_TS["Y_RIGHT_WRIST"]
guesser_right_wrist_TS = guesser_body_TS["Y_RIGHT_WRIST"]

# Y_NOSE 
clue_giver_nose_TS = clue_giver_body_TS["Y_NOSE"]
guesser_nose_TS = guesser_body_TS["Y_NOSE"]

# Average of left and right shoulder x-coordinates
clue_giver_shoulder_TS = (clue_giver_body_TS["X_LEFT_SHOULDER"] + clue_giver_body_TS["X_RIGHT_SHOULDER"]) / 2
guesser_shoulder_TS = (guesser_body_TS["X_LEFT_SHOULDER"] + guesser_body_TS["X_RIGHT_SHOULDER"]) / 2
```

As with the hip data, let's manually apply linear detrending + z-scoring just for visualization purposes:

```python
def plot_time_series(ts1, ts2, time, title='Time Series Plot', yaxis_label='Value'):

    # Manually apply linear detrending and z-scoring to the time series, 
    # only for visualization purposes (this is applied separately in pyspi)
    ts1_detrend = scipy.stats.zscore(scipy.signal.detrend(ts1))
    ts2_detrend = scipy.stats.zscore(scipy.signal.detrend(ts2))

    # Combine the two time series into a two-column array
    both_TS = pd.DataFrame({
        'Clue-giver': ts1_detrend,
        'Guesser': ts2_detrend,
        'Time': time

    })

    # Plot the time series
    plt.figure(figsize=(10, 5))
    both_TS.plot(x='Time', y=['Clue-giver', 'Guesser'])

    ax = plt.gca()
    ax.legend(labels=['Clue-giver', 'Guesser'], loc='center left', bbox_to_anchor=(1, 0.5))
    plt.xlabel('Time (ms)')
    plt.ylabel(yaxis_label)
    plt.title(title)
    plt.show()

# Y_RIGHT_WRIST
plot_time_series(clue_giver_right_wrist_TS, guesser_right_wrist_TS, clue_giver_body_TS['time'], 
                 title='Right wrist', yaxis_label='Right Wrist y-coordinate (z-score)')

# Y_NOSE
plot_time_series(clue_giver_nose_TS, guesser_nose_TS, clue_giver_body_TS['time'], 
                 title='Nose', yaxis_label='Nose y-coordinate (z-score)')

# Average of left and right shoulder x-coordinates
plot_time_series(clue_giver_shoulder_TS, guesser_shoulder_TS, clue_giver_body_TS['time'], 
                 title='Shoulders', yaxis_label='Shoulder x-coordinate (z-score)')
```

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Now let's compute the `sonnet` SPI subset for each of the four body-part pairs, using a small helper function so we don't repeat the reshaping logic four times over:

```python
# Apply the 'sonnet' SPI subset to each pair of time series
def apply_pyspi_subset(ts1, ts2):
    data = Data(np.stack([ts1, ts2]), detrend=True, normalise=True)
    calc = Calculator(dataset=data, subset='sonnet')
    calc.compute()
    table = calc.table
    table.columns = table.columns.to_flat_index()
    table = table.rename(columns='__'.join).assign(role_from = lambda x: x.index)
    table_long = table.melt(id_vars='role_from', var_name='SPI__role_to', value_name='value')
    table_long["SPI"] = table_long["SPI__role_to"].str.split("__").str[0]
    table_long["role_to"] = table_long["SPI__role_to"].str.split("__").str[1]
    table_long = (table_long.assign(role_from = lambda x: np.where(x.role_from == 'proc-0', 'Clue-giver', 'Guesser'),
                                    role_to = lambda x: np.where(x.role_to == 'proc-0', 'Clue-giver', 'Guesser'))
                    .drop(columns=['SPI__role_to'])
                    .query("role_from != role_to")
                    )
    return table_long

# Average of left and right hip x-coordinates
hip_pyspi_results = apply_pyspi_subset(clue_giver_hip_TS, guesser_hip_TS)
hip_pyspi_results['body_part'] = 'Hips'

# Y_RIGHT_WRIST
right_wrist_pyspi_results = apply_pyspi_subset(clue_giver_right_wrist_TS, guesser_right_wrist_TS)
right_wrist_pyspi_results['body_part'] = 'Right Wrist'

# Y_NOSE
nose_pyspi_results = apply_pyspi_subset(clue_giver_nose_TS, guesser_nose_TS)
nose_pyspi_results['body_part'] = 'Nose'

# Average of left and right shoulder x-coordinates
shoulder_pyspi_results = apply_pyspi_subset(clue_giver_shoulder_TS, guesser_shoulder_TS)
shoulder_pyspi_results['body_part'] = 'Shoulders'
```

Combining all four body-part results into one long-format table lets us compare SPI values across body parts directly, faceted by statistic:

```r
%%R -i hip_pyspi_results,right_wrist_pyspi_results,nose_pyspi_results,shoulder_pyspi_results -w 900 -h 800

# Combine all body part results into a single data frame
combined_results <- rbind(hip_pyspi_results, right_wrist_pyspi_results, nose_pyspi_results, shoulder_pyspi_results)

combined_results %>%
    filter(role_from == 'Clue-giver') %>%
    ggplot(data=., mapping=aes(x=body_part, y=value, fill=body_part)) +
    geom_bar(stat="identity", position=position_dodge()) +
    facet_wrap('SPI', scales='free', ncol=3)  +
    ylab('SPI value') +
    xlab('Body part') +
    labs(fill= 'Body part') +
    geom_hline(yintercept=0, linetype="dashed", color = "black") +
    ggtitle('SPI values for different body parts (Clue-giver to Guesser)') +
    theme(axis.text.x = element_blank(),
          axis.ticks.x = element_blank(),
          plot.title = element_text(hjust = 0.5),
          legend.position = 'bottom')
```

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

***

## :star: Level 5: Sonnet subset across a 4-node hip + nose system

So far every SPI has been computed on an isolated _pair_ of time series. Level 5 instead builds a single 4-node multivariate dataset — both hips and both noses — and computes the `sonnet` subset once across all node pairs, giving a full 4x4 interaction matrix for each statistic.

```python
# Combine all body part results into a single numpy array
hip_and_nose_combined_TS = np.stack([clue_giver_hip_TS, guesser_hip_TS, 
                                     clue_giver_nose_TS, guesser_nose_TS], axis=0)

# Convert to pyspi Data object, applying linear detrending and z-scoring to each time series
hip_and_nose_combined_data = Data(hip_and_nose_combined_TS, detrend=True, normalise=True, dim_order='ps')

# Apply the 'sonnet' SPI subset to the combined data
calc_hip_and_nose_combined = Calculator(dataset=hip_and_nose_combined_data, subset='sonnet')
calc_hip_and_nose_combined.compute()
```

Let's check which SPIs were computed:

```python
calc_hip_and_nose_combined.spis.keys()
```

**Output:**

```
dict_keys(['cov_EmpiricalCovariance', 'dtw_constraint-itakura', 'bary_dtw_mean', 'anm', 'di_gaussian', 'te_kraskov_NN-4_DCE_k-max-10_tau-max-4', 'phi_star_t-1_norm-0', 'cohmag_multitaper_mean_fs-1_fmin-0_fmax-0-5', 'psi_multitaper_mean_fs-1_fmin-0_fmax-0-5', 'pli_multitaper_max_fs-1_fmin-0_fmax-0-5', 'sgc_nonparametric_mean_fs-1_fmin-0_fmax-0-5', 'psi_wavelet_mean_fs-1_fmin-0_fmax-0-5_mean', 'coint_aeg_tstat_trend-ct_autolag-aic_maxlag-10', 'pec'])
```

For a 4-node system it's easier to read each SPI as a 4x4 interaction matrix rather than a bar chart. The helper below plots one matrix per SPI, using a diverging colormap when the statistic can go negative and a sequential one otherwise:

```python
def plot_mpi(S,identifier,labels,ax=None):
    """ Plot a given matrix of pairwise interactions, annotating the process labels and identifier
    """
    if ax is None:
        _, ax = plt.subplots()
    plt.sca(ax)

    # Use a diverging cmap if our statistic goes negative (and a sequential cmap otherwise)
    if np.nanmin(S) < 0.:
        maxabsval = max(abs(np.nanmin(S)),abs(np.nanmax(S)))
        norm = colors.Normalize(vmin=-maxabsval, vmax=maxabsval)
        plt.imshow(S,cmap='coolwarm',norm=norm)
    else:
        plt.imshow(S,cmap='Reds',vmin=0)

    plt.xticks(ticks=range(len(labels)),labels=labels,rotation=90)
    plt.yticks(ticks=range(len(labels)),labels=labels)
    plt.xlabel('Target Body Part')
    plt.ylabel('Source Body Part')
    plt.title(identifier)
    plt.colorbar()

# Time-series labels
ts_labels = ['Clue-giver Hip', 'Guesser Hip',
             'Clue-giver Nose', 'Guesser Nose']

# Iterate over SPIs and plot the 4x4 result matrix
for identifier in calc_hip_and_nose_combined.spis.keys():
    S = calc_hip_and_nose_combined.table[identifier]
    plot_mpi(S, identifier, ts_labels)
```

**Output:**

{% columns %}
{% column %}
<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>


{% endcolumn %}

{% column %}
<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>


{% endcolumn %}
{% endcolumns %}

## :brain: An example from real brain-data analysis

To show that the same `pyspi` workflow generalizes beyond motion-tracking data, we close with a quick example on real resting-state fMRI data: computing the `sonnet` SPI subset between homotopic (left/right hemisphere) pairs of brain regions from the Desikan-Killiany parcellation.

Here's what that parcellation atlas looks like:

```r
%%R -w 800 -h 450

ggplot() +
  geom_brain(atlas = atlas_region_remove(dk(), c("corpuscallosum")), 
             hemi = 'left', view = c('medial', 'lateral'), 
             show.legend = TRUE, linewidth = 0.5, colour = 'black') +
  theme_void() +
  labs(fill='Brain region') +
  theme(legend.position = "bottom") 
```

Let's look at the (already z-scored) time series for three randomly sampled brain regions:

```python
rsfMRI_TS = pd.read_csv('https://github.com/anniegbryant/MDIG2026_pyspi_tutorial/raw/refs/heads/main/HCP_298051_rsfMRI_DesikanKilliany_TS.csv')
```

You can also access the file by downloading from [here](https://github.com/anniegbryant/MDIG2026_pyspi_tutorial/tree/main).

```r
%%R -i rsfMRI_TS -w 1200 -h 400

set.seed(127) # For reproducibility
random_sample_regions <- sample(colnames(rsfMRI_TS)[1:34], 4)

rsfMRI_TS %>% 
    select(all_of(random_sample_regions)) %>%
    mutate(time_sec = 0.7*1:n()) %>%
    filter(time_sec <= 120) %>%
    pivot_longer(cols = -time_sec, names_to = "region", values_to = "value") %>%
    mutate(region = str_replace(region, 'ctx-lh-', '')) %>%
    ggplot(data=., mapping=aes(x=time_sec, y=value, color=region)) +
    geom_line() +
    scale_x_continuous(expand=c(0,0)) +
    ggtitle("First 120 seconds of rsfMRI BOLD signal from 4 randomly selected brain regions") +
    xlab("Time (s)") +
    ylab("BOLD signal (z-score)") +
    theme(legend.position = "bottom",
          axis.text = element_text(size=14),
          axis.title = element_text(size=16),
          legend.text = element_text(size=15),
          legend.title = element_text(size=16),
          plot.title = element_text(hjust = 0.5, size=18)) 
```

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

For each unique region in the parcellation, we pull out its left- and right-hemisphere time series, build a `pyspi` `Data` object from the pair, and compute the `sonnet` subset, looping over all unique regions and collecting the results into one long-format table:

```python
# Create a new Calculator object
base_calc = Calculator(subset='sonnet')

# Read in brain regions
brain_region_lookup = pd.read_csv("https://github.com/anniegbryant/MDIG2026_pyspi_tutorial/raw/refs/heads/main/Brain_Region_info.csv", 
                                  index_col=False).reset_index(drop=True)
base_regions = list(set(brain_region_lookup.Base_Region.tolist()))

# Create list to store homotopic FC results for each base region
homotopic_FC_results_list = []

# Iterate over the base regions as the target regions
for this_region in base_regions:
    region_info_subset = brain_region_lookup.query("Base_Region == @this_region")
    left_region_name = region_info_subset.query("Hemisphere == 'Left'")["Brain_Region"].values[0]
    right_region_name = region_info_subset.query("Hemisphere == 'Right'")["Brain_Region"].values[0]

    # Extract the time series for the left and right regions
    left_region_TS = rsfMRI_TS[left_region_name].values
    right_region_TS = rsfMRI_TS[right_region_name].values

    base_region_data = Data(np.stack([left_region_TS, right_region_TS]), detrend=True, normalise=True, dim_order='ps')

    # Make a copy of the base Calculator object
    calc = deepcopy(base_calc)

    # Load the brain region pair data
    calc.load_dataset(base_region_data)

    # Compute SPIs
    calc.compute()

    # Get SPI results
    homotopic_FC_res = deepcopy(calc.table)
    homotopic_FC_res.columns = homotopic_FC_res.columns.to_flat_index()

    homotopic_FC_res = homotopic_FC_res.rename(columns='__'.join).assign(hemi_from = lambda x: x.index)
    homotopic_FC_res_long = homotopic_FC_res.melt(id_vars='hemi_from', var_name='SPI__hemi_to', value_name='value')

    # Reshape from wide to long, and add columns for hemisphere and base region
    homotopic_FC_res_long["SPI"] = homotopic_FC_res_long["SPI__hemi_to"].str.split("__").str[0]
    homotopic_FC_res_long["hemi_to"] = homotopic_FC_res_long["SPI__hemi_to"].str.split("__").str[1]

    homotopic_FC_res_long = (homotopic_FC_res_long.assign(hemi_from = lambda x: np.where(x.hemi_from == 'proc-0', 'L', 'R'),
                                        hemi_to = lambda x: np.where(x.hemi_to == 'proc-0', 'L', 'R'))
                        .drop(columns=['SPI__hemi_to'])
                        .query("hemi_from != hemi_to")
                        .assign(Base_Region = this_region)
                        )
    
    homotopic_FC_results_list.append(homotopic_FC_res_long)

# Concatenate all the results into a single DataFrame
all_homotopic_FC_results = pd.concat(homotopic_FC_results_list, ignore_index=True)
```

A peek at the resulting long-format table of homotopic SPI values:

```python
all_homotopic_FC_results.head()
```

| index | hemi\_from | value     | SPI                      | hemi\_to | Base\_Region |
| ----- | ---------- | --------- | ------------------------ | -------- | ------------ |
| 0     | R          | 0.513826  | cov\_EmpiricalCovariance | L        | fusiform     |
| 1     | L          | 0.513826  | cov\_EmpiricalCovariance | R        | fusiform     |
| 2     | R          | 18.860660 | dtw\_constraint-itakura  | L        | fusiform     |
| 3     | L          | 18.860660 | dtw\_constraint-itakura  | R        | fusiform     |
| 4     | R          | -0.000524 | bary\_dtw\_mean          | L        | fusiform     |

The standard method for examining homotopic connectivity would be the Pearson correlation coefficient (`cov_EmpiricalCovariance` in pyspi, since covariance with -scored time series is the Pearson correlation).

Let's compare the behavior of the Pearson correlation coefficient with that of the power envelop correlation (`pec`; highly correlated) and the mean of the barycenter with dynamic time warping alignment (`bary_dtw_mean`; minimally correlated):

```r
%%R -i all_homotopic_FC_results -w 800 -h 350

all_homotopic_FC_results %>%
    filter(SPI %in% c('cov_EmpiricalCovariance', 'pec', 'bary_dtw_mean') & hemi_from=='L') %>% 
    pivot_wider(names_from = SPI, values_from = value) %>%
    pivot_longer(cols=c('pec', 'bary_dtw_mean'), names_to = 'SPI_2', values_to = 'value') %>%
    ggplot(data=.,  mapping=aes(x=cov_EmpiricalCovariance, y=value, color=SPI_2)) +
    geom_point(alpha=0.75, stroke=0, size=3) +
    facet_wrap(~SPI_2, scales='free', ncol=2) +
    ylab('SPI value') +
    xlab('Pearson correlation coefficient') +
    scale_color_manual(values=c('pec'='#83BC7A', 'bary_dtw_mean'='#2D6932')) +
    theme(legend.position='none',
          strip.background = element_blank(),
          strip.text = element_text(size=14, face='bold'),
          axis.title = element_text(size=14))
```

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

We can visualize these three SPIs, all of which are undirected, projected onto the cortical surface to appreciate their spatial distribution:

```r
%%R -i all_homotopic_FC_results,brain_region_lookup -w 700 -h 450


df <- all_homotopic_FC_results %>%
    filter(SPI %in% c('cov_EmpiricalCovariance', 'pec', 'bary_dtw_mean') & hemi_from=='L') %>% 
    select(Base_Region, SPI, value) %>%
    left_join(brain_region_lookup %>% filter(Hemisphere=='Left'), by=c("Base_Region"="Base_Region")) %>%
    rename('label' = 'ggseg') %>%
    mutate(label = str_replace(label, 'ctx-lh-', 'lh_')) %>%
    left_join(as.data.frame(dk()), by=c("label"="label"), relationship = "many-to-many") 

# Use Desikan-Killiany atlas to visualize the results, removing the corpus callosum region for clarity
dk_atlas <- dk()
dk_atlas <- atlas_region_remove(dk_atlas, c("corpuscallosum"))

# Start list of brain plots
brain_plot_list <- list()

# Iterate over SPIs
for (spi in unique(df$SPI)) {
    df_spi <- df %>% filter(SPI == spi)
    
    p <- ggplot() +
        geom_brain(atlas = dk_atlas, mapping=aes(fill=value), data = df_spi,
                hemi = 'left', view = c('medial', 'lateral'),
                linewidth = 0.5, colour = 'black') +
        ggtitle(paste("SPI:", spi)) +
        scale_fill_gradientn(colors=c('blue', 'white', 'red'), na.value = 'grey90') +
        theme_void() +
        theme(plot.title = element_text(hjust = 0.5, size=14, face='bold'))
    
    brain_plot_list[[spi]] <- p
}

# Combine the plots into a single figure using patchwork
wrap_plots(brain_plot_list, ncol=1)
```

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>
