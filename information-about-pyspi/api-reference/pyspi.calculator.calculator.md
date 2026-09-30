# pyspi.calculator.Calculator

> _<mark style="color:blue;">class</mark>_ **pyspi.calculator.Calculator**(_<mark style="color:blue;">dataset=None</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">name=None</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">labels=None</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">fast=False</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">sonnet=False</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">configfile=None, detrend=False, normalise=True</mark>)_

**Compute all pairwise interactions.**

The calculator takes in a multivariate time-series dataset (MTS), computes and stores all pairwise interactions for the dataset. It uses a YAML configuration file that can be modified in order to compute a reduced set of pairwise methods.

### Example

```python
import numpy as np

dataset = np.random.randn(5,500)   # create a random multivariate time series (MTS)
calc = Calculator(dataset=dataset) # Instantiate the calculator
calc.compute()                     # Compute all pairwise interactions
```

<table><thead><tr><th width="145">Parameters</th><th>Description</th></tr></thead><tbody><tr><td></td><td><ul><li><strong>dataset</strong> (<a href="pyspi.data.data.md"><code>Data</code></a>, array_like, optional) – The multivariate time series of M processes and T observations, default=None</li><li><strong>name</strong> (<a href="https://docs.python.org/3/library/stdtypes.html#str"><em>str</em></a><em>, optional</em>) – The name of the calculator. Mainly used for printing the results but can be useful if you have multiple instances, default=None.</li><li><strong>labels</strong> (<em>array_like, optional</em>) – Any set of strings by which you want to label the calculator. This can be useful later for classification purposes, default=None.</li><li><strong>subset</strong> (<a href="https://docs.python.org/3/library/stdtypes.html#str"><em>str</em></a>, <em>optional</em>) - A pre-configured subset of SPIs to use. Options are "all", "fast", "sonnet", "octaveless", or "fabfour", default="all".</li><li><strong>configfile</strong> (<a href="https://docs.python.org/3/library/stdtypes.html#str"><em>str</em></a><em>, optional</em>) – The location of the YAML configuration file. See <a href="../../installing-and-using-pyspi/usage/advanced-usage/creating-a-reduced-spi-set.md">Using a reduced SPI set</a>, defaults to <mark style="color:red;"><code>'&#x3C;/path/to/pyspi>/pyspi/config.yaml'</code></mark></li><li><strong>detrend</strong> <em>(<mark style="color:blue;">bool</mark>, optional)</em>  - Detrend the dataset along the time axis before normalising (if enabled), default=False.</li><li><strong>normalise</strong> <em>(<mark style="color:blue;">bool</mark>, optional) -</em> Z-score normalise the dataset along the time axis before computing SPIs, default=True..</li></ul></td></tr></tbody></table>

> **`__init__`**(_dataset=None, name=None, labels=None, subset=None, configfile=None, detrend = False, normalise=True)_

## Methods

<table><thead><tr><th width="417">Method</th><th>Description</th></tr></thead><tbody><tr><td><code>__init__</code>([dataset, name, labels, fast, ...])</td><td></td></tr><tr><td><code>compute</code>()</td><td>Compute the SPIs on the MVTS dataset.</td></tr><tr><td><code>load_dataset</code>(dataset)</td><td>Load a new dataset into existing instance.</td></tr><tr><td><code>set_group</code>(classes)</td><td>Assigns a numeric value to a Calculator instance based on a list of classes.</td></tr><tr><td><code>_rmin</code>()</td><td>Iterate through all SPIs are remove the minimum. Fixes absolute errors when correlating. </td></tr><tr><td><code>get_stat_labels</code>()</td><td>Get the <a href="../spis/glossary-of-terms.md#keywords">keywords</a> for each SPI.</td></tr><tr><td><code>_get_correlation_df</code>(with_labels=False, rmin=False)</td><td>Generates a DataFrame showing correlations between SPIs. </td></tr></tbody></table>

## Attributes

<table><thead><tr><th width="194">Attribute</th><th>Description</th></tr></thead><tbody><tr><td><code>dataset</code></td><td>Dataset as a data object.</td></tr><tr><td><code>group</code></td><td>The numerical group assigned during <code>set_group()</code></td></tr><tr><td><code>group_name</code></td><td>The group name assigned during <code>set_group()</code>.</td></tr><tr><td><code>labels</code></td><td>List of calculator labels.</td></tr><tr><td><code>n_spis</code></td><td>Number of SPIs in the calculator.</td></tr><tr><td><code>name</code></td><td>Name of the calculator.</td></tr><tr><td><code>spis</code></td><td>Dict of SPIs.</td></tr><tr><td><code>table</code></td><td>Results table for all pairwise interactions (each represented as an MPI).</td></tr></tbody></table>





