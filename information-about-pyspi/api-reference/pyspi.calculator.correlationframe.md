# pyspi.calculator.CorrelationFrame

> _<mark style="color:blue;">class</mark>_ **pyspi.calculator.CorrelationFrame**(_<mark style="color:blue;">cf=None,  \*\*kwargs</mark>_)

Container for handling correlation data derived from a set of calculators.&#x20;

## Example

```python
from pyspi.calculator import CorrelationFrame, CalculatorFrame
from pyspi.calculator import Calculator

# create the calculator frame using your data
calc_frame = CalculatorFrame(calculators=[Calculator(dataset=dataset), 
    Calculator(dataset=dataset2)]
    
# create the correlation frame
corr_frame = CorrelationFrame(cf=calc_frame)

```



<table><thead><tr><th width="252">Parameters</th><th>Description</th></tr></thead><tbody><tr><td></td><td><ul><li><strong>cf</strong> (<a href="pyspi.calculator.calculatorframe.md">CalculatorFrame</a>, optional) - An instance of a CalculatorFrame or Caclulator object.</li><li><strong>kwargs</strong> (<em>dict</em>, <em>optional</em>) - Additional keyword arguments for CorrelationFrame initialisation. </li></ul></td></tr></tbody></table>

> **`__init__`**(_cf=None, \*\*kwargs_)

## Methods

<table><thead><tr><th width="395">Attribute</th><th>Description</th></tr></thead><tbody><tr><td><code>__</code><strong><code>init__</code></strong><code>(cf=None, **kwargs)</code></td><td>Constructor for initialising the CorrelationFrame object. Accepts a <a href="pyspi.calculator.calculatorframe.md">CalculatorFrame</a> ('cf') and additional keyword arguments. </td></tr><tr><td><code>get_pvalues()</code></td><td>-</td></tr><tr><td><code>merge()</code></td><td>Merges the current CorrelationFrame with another.</td></tr><tr><td><code>compute_significant_values()</code></td><td>-</td></tr><tr><td><code>get_average_correlation</code>(thresh=0.2, absolute=True, summary="mean", remove_insig=False)</td><td>Calculates and returns the average correlation, with various options for thresholding, absolutes and removing insignificant values. </td></tr><tr><td><code>get_feature_matrix</code>(sthresh=0.8, dthresh=0.2, dropduplicates=True)</td><td>Returns feature matrix based on specified thresholds and duplication settings. </td></tr><tr><td><code>relabel_spis</code>(names, labels)</td><td>-</td></tr><tr><td><code>set_sgroup_names</code>(names=None)</td><td>-</td></tr><tr><td></td><td></td></tr></tbody></table>

## Attributes



| Method    | Description                           |
| --------- | ------------------------------------- |
| `name`    | Name of the CorrelationFrame object.  |
| `dlabels` | -                                     |



