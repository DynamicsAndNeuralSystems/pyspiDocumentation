---
description: Description of all pre-defined subsets in pyspi.
coverY: 0
---

# SPI Subsets

As standard, we include several SPI subsets in _pyspi_ that can be used directly when instantiating a [`Calculator`](../api-reference/pyspi.calculator.calculator.md). Below we provide details about each subset including its intended purpose.&#x20;

<table><thead><tr><th width="186">Subset </th><th width="130"># of SPIs</th><th>Description</th></tr></thead><tbody><tr><td><code>fabfour</code></td><td><code>4</code></td><td>Four basic pairwise measures that are commonly used in the literature. </td></tr><tr><td><code>sonnet</code></td><td><code>14</code></td><td>A minimally redundant set obtained by grouping SPIs according to their empirical behaviour on over 1000 MTS. Further details are provided in this <a href="https://arxiv.org/pdf/2201.11941.pdf">paper</a>. We also provide<a href="table-of-spis.md#short-names"> short names</a> for each of the features in this subset. </td></tr><tr><td><code>fast</code></td><td><code>216</code></td><td>A subset of SPIs that are fastest to compute. Provides an ideal balance between comprehensiveness of pairwise measures and computational demand. </td></tr><tr><td><code>all</code></td><td><code>284</code></td><td><strong>Default</strong> option. Contains the entire library of SPIs.</td></tr></tbody></table>

### Subset Usage

To specify a subset, simply initialise the [`Calculator`](../api-reference/pyspi.calculator.calculator.md) using the subset name as a parameter:

```python
from pyspi.calculator import Calculator
import numpy as np 

dat = np.random.randn(2, 100)
calc = Calculator(dataset=dat, subset='fast')
calc.compute()
```
