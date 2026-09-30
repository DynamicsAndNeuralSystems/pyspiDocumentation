---
cover: >-
  https://images.unsplash.com/photo-1555066931-4365d14bab8c?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw2fHxjb2RlfGVufDB8fHx8MTcxMDUzODA2NHww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Creating a reduced SPI set

## Filtering an SPI Set

If you would like to filter SPIs based on their corresponding keywords, you can do so using the **`filter_spis`** function in _pyspi_. This will return a new config file containing the reduced SPI set that you can provide to the `Calculator` object.&#x20;

As an example, let's use the filter to obtain a subset of SPIs with the keywords `linear` and `signed`:

```python
from pyspi.utils import filter_spis

# let's only select SPIs with the keywords "linear" and "signed"
# we will need to provide these keywords in a list
keywords = ["linear", "signed"]

# we will need to specify the location of the original config.yaml
filter_spis(keywords=keywords, output_name="linear_signed.yaml")
```

_Note_ that here we do not specify a source config.yaml file. By **default**, the `filter_spis` function will use the **original config.yaml** (containing the full set of SPIs) located in the _pyspi_ directory.  If you would like to provide a custom YAML as the source file, you can provide the location of the file to the function with the optional keyword `configfile.`

<details>

<summary>[OPTIONAL] Filtering from a user-specified YAML</summary>

Here, we tell the filter function to use a custom yaml file `myconfig.yaml` located in the current working directory:

```python
filter_spis(keywords=keywords, output_name="linear_signed.yaml", configfile="./myconfig.yaml") 
```

You must ensure that the custom config YAML conforms to the structure of a _pyspi_ YAML otherwise the filter function will not work as expected. See [manually specifying a reduced set ](creating-a-reduced-spi-set.md#manually-specifying-a-reduced-set)for more information about the structure _pyspi_ expects.

</details>

We should obtain a reduced subset of SPIs with the keywords "linear" and "signed". Let's now compute this reduced set on some data:

```python
from pyspi.calculator import Calculator
import numpy as np

data = np.random.randn(3, 100) # your time series data
calc = Calculator(configfile="linear_signed.yaml", dataset=data)

# print the number of SPIs initialised in the Calculator instance
print(calc.n_spis)
```

***

## Manually specifying a reduced set

You can use a subset of the SPIs by copying a version of the <mark style="color:red;">**`config.yaml`**</mark> file to a local directory and manually removing those you don’t want the calculator to compute. First, copy the <mark style="color:red;">**`config.yaml`**</mark> file to your workspace:

```python
import os, shutil
import pyspi

# chose the location where you would like to save the copy 
destination = "./myconfig.yaml"
# get the pyspi directory where config.yaml is located
source = os.path.dirname(pyspi.__file__) + "/config.yaml"
shutil.copy(source, destination)
```

Once you've got a local copy of <mark style="color:red;">**`config.yaml`**</mark> edit the file to remove any SPIs you're not interested in. A minimal configuration file might look like the following if you're only interested in computing a covariance matrix using the _maximum likelihood estimator_:

{% code title="myconfig.yaml" %}
```yaml
# Basic statistics
.statistics.basic:
    # Covariance
    covariance:
       labels:
          - undirected
          - linear
          - signed
          - multivariate
          - contemporaneous
       dependencies:
       configs:
        # Maximum likehood estimator
        - estimator: EmpiricalCovariance
```
{% endcode %}

Ensure you retain the `labels` and `dependencies` keys from the original <mark style="color:red;">**config.yaml**</mark> file when creating your custom set.&#x20;

When you instantiate the calculator, instead of using the default <mark style="color:red;">**`config.yaml,`**</mark> you can input your bespoke configuration file:

```python
from pyspi.calculator import Calculator

calc = Caculator(dataset=dataset, configfile='myconfig.yaml')
```

Then use the calculator as normal (see [Getting Started](../walkthrough-tutorials/getting-started-a-simple-demonstration.md)).&#x20;
