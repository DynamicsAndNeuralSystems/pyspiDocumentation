# pyspi.calculator.CalculatorFrame

> _<mark style="color:blue;">class</mark>_ **pyspi.calculator.CalculatorFrame**_<mark style="color:blue;">(calculators=None, name=None, datasets=None, names=None, labels=None)</mark>_

Container for batch level commands such as computing/pruning/initialising multiple datasets at once.

### Example

```python
from pyspi.calculator import CalculatorFrame
from pyspi.calculator import Calculator
from pyspi.data import Data

# provide dataset filepaths
datsets = ['../pyspi/data/cml.npy', '../pyspi/data/forex.npy', '../pyspi/data/standard_normal.npy']
# provide names for each dataset
dataset_names = ['cml', 'forex', 'standard_normal']
# optionally, provide labels for each dataset/calculator
dataset_labels = ['label1', 'label2', 'label3']

# create calculator frame
calc_frame = CalculatorFrame(
    name="MyCalculatorFrame",
    datasets = [Data(data=data, dim_order = 'sp') for data in datasets],
    names=dataset_names,
    labels=dataset_labels,
    subset = 'fast' # pass the subset parameter along to the calculators
)

# run compute on all calculators in the frame
calc_frame.compute()

# access the results table for dataset 1
calc_frame.calculators[0][0].table

# get correlation frame
cf = calc_frame.get_correlation_df()

```

***

<table><thead><tr><th width="296">Parameters</th><th>Description</th></tr></thead><tbody><tr><td></td><td><ul><li><strong>calculators</strong> - Initial set of calculators to be added to the frame. </li><li><strong>name</strong> (<a href="https://docs.python.org/3/library/stdtypes.html#str"><em>str</em></a><em>, optional</em>) - A name for the CalculatorFrame instance. </li><li><strong>datasets</strong> (<em>list</em>, optional) - A list of datasets to initialise calculators with.</li><li><strong>names</strong> (<em>list of str,</em> <em>optional</em>) - Corresponding names for each dataset in 'datasets'. </li><li><strong>labels</strong> (<em>list</em>, <em>optional</em>) - Labels for the datasets in 'datasets'.</li><li><strong>kwargs</strong> (<em>dict</em>, <em>optional</em>) - Additional keyword arguments for calculator initialisation. </li></ul></td></tr></tbody></table>

> `__init__`(calculators=None, name=None, datasets=None, names=None, labels=None, \*\*kwargs)

## Methods

| Method                                                                                          | Description                                                                 |
| ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| `from_calculator`(calculator)                                                                   | Creates a CalculatorFrame from a single calculator.                         |
| `set_calculator`(calculators)                                                                   | Sets the calculator(s) for the frame.                                       |
| `add_calculator`(calc)                                                                          | Adds a calculator to the existing frame.                                    |
| `init_from_list`(datasets, names, labels, \*\*kwargs)                                           | Initialises calculators from a list of datasets.                            |
| `init_from_yaml`(document, normalise=True, n\_processes=None, n\_observations=None, \*\*kwargs) | Initialises calculators from a YAML file.                                   |
| `merge`(other)                                                                                  | Merges another CalculatorFrame's calculations into the current frame.       |
| `compute`(calc)                                                                                 | Computes results for each calculator object.                                |
| `set_group`(calc, \*args)                                                                       | Sets the group for each calculator.                                         |
| `get_correlation_df`(with\_labels=False, \*\*kwargs)                                            | Generates a DataFrame showing correlations between SPIs for each dataset.   |

## Attributes

| Attribute | Description                      |
| --------- | -------------------------------- |
| `name`    | The name of the CalculatorFrame. |
