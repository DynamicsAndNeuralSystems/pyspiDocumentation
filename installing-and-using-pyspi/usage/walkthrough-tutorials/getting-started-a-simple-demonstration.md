---
cover: ../../../.gitbook/assets/fahrul-razi-BR6lrzCPYPk-unsplash.jpg
coverY: 0
---

# Getting Started: A Simple Demonstration

## Simple Demonstration

Now that you have installed _pyspi,_ let's get started with a very simple demonstration of how we would apply it to a dataset of interest. Here we will generate a generic dataset from a multivariate Gaussian distribution:

{% code fullWidth="false" %}
```python
import numpy as np
import random

random.seed(42)

M = 5 # 5 independent processes
T = 500 # 500 samples per process

dataset = np.random.randn(M,T) # generate our multivariate time series
```
{% endcode %}

**Trial run with a reduced set**:

As good practice, we always recommend doing a trial run of _pyspi_ with a smaller subset of SPIs first (e.g., the [_fabfour_](../../../information-about-pyspi/spis/spi-subsets.md), [_sonnet_](../../../information-about-pyspi/spis/spi-subsets.md), or [_fast_](../../../information-about-pyspi/spis/spi-subsets.md) sets). In this way, we can run _pyspi_ on our data quickly, identifying and resolving any potential issues before proceeding with more computationally intensive calculations. Once you are satisfied with the analysis pipeline, you can always scale up to the full _pyspi_ library of over 250 SPIs.  Let's do a trial run with the 'fast' subset by instantiating the `Calculator` object in the following way:

```python
from pyspi.calculator import Calculator

calc = Calculator(dataset=dataset, subset='fast') # instantiate the calculator object
```

Note that by default, your dataset will be **normalised** before computing SPIs. If you would like to disable this pre-processing step, pass the `normalise=False` flag to the Calculator object when instantiating. For more information, see the [API docs](../../../information-about-pyspi/api-reference/pyspi.calculator.calculator.md).

```python
calc = Calculator(dataset=dataset, subset='fast', normalise=False)
```

After successfully initialising the Calculator, you should see the summary of the pre-processing steps applied to the data for verification:

```
216 SPI(s) were successfully initialised.

[1/2] Skipping detrending of the dataset...
[2/2] Normalising (z-scoring) the dataset...
```

Now that we have passed our dataset into the [Calculator](../../../information-about-pyspi/api-reference/pyspi.calculator.calculator.md) object, verified the pre-procesing steps we require, and specified which SPIs we would like to compute via the subset parameter, we can now compute all of our SPIs by simply calling the compute method:&#x20;

```python
calc.compute()
```

Once the calculator has computed each of the statistics, you can access all SPI outputs using the table property:

```python
print(calc.table)
```

Alternatively, if we would like to examine a specific method's outputs, we can extract its corresponding matrix of pairwise interactions ([MPI](../../../information-about-pyspi/spis/glossary-of-terms.md)) by specifying its unique identifier.  For instance, the following code will extract the covariance matrix computed with the maximum likelihood estimator:

```python
print(calc.table['cov_EmpiricalCovariance'])
```

**Using the full SPI set**:

By default, _pyspi_ will instantiate a Calculator with the full _pyspi set (_&#x61;ll SPI&#x73;_)._ To access the full _pyspi_ library of SPIs, simply call the Calculator as follows either without specifying a subset **or** passing `subset = 'all':`

<pre class="language-python"><code class="lang-python">calc = Calculator(dataset=dataset) # use all SPIs
# alterantively, one can specify the subset 'all'
<strong>calc = Calculator(dataset=dataset, subset='all')
</strong>
# compute the SPIs as usual
calc.compute()
</code></pre>

While we tried to make the calculator as efficient as possible, computing all statistics can take a while (depending on the size of your dataset). To give users a sense of how long the full _pyspi_ set takes to run, see the [FAQ](../faq.md) section.

{% hint style="info" %}
**Distributed Computing**&#x20;

Given the intensive nature of calculations with the full SPI set, you might want to explore options for distributed computing, particularly if you are working with large datasets. For further details on how you can get started with running _pyspi_ on a HPC cluster, refer to the [Advanced Usage section](../advanced-usage/distributing-calculations-on-a-cluster.md).&#x20;
{% endhint %}

***

## Bonus: Construct a custom SPI subset

In this simple tutorial, we have demonstrated how you can use reduced subsets of SPIs in your analysis. Subsets such as '[fabfour](../../../information-about-pyspi/spis/spi-subsets.md)', '[sonnet](../../../information-about-pyspi/spis/spi-subsets.md)' or '[fast](../../../information-about-pyspi/spis/spi-subsets.md)' offer a streamlined way to ensure your pipeline is working as expected, without overwhelming computational demands. However there might be scenarios where these predefined subsets don't align with your specific needs or objectives. In such cases, creating a custom subset of SPIs may be highly advantageous.&#x20;

If you would like to construct your own reduced subset of SPIs, follow the guide in the [Advanced Usage section](../advanced-usage/creating-a-reduced-spi-set.md) to get started. We also recommend checking the [table of SPIs](../../../information-about-pyspi/spis/table-of-spis.md) and the [detailed SPI descriptions](../../../information-about-pyspi/spis/spi-descriptions/) to help guide your selection of SPIs when creating your own custom subsets.&#x20;
