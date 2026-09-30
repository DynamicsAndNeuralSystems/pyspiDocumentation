# pyspi.data.Data

> _<mark style="color:blue;">class</mark>_  **pyspi.data.Data**<mark style="color:blue;">(</mark>_<mark style="color:blue;">data=None</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">dim\_order='ps'</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">detrend=False</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">normalise=True</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">name=None</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">procnames=None</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">n\_processes=None</mark>_<mark style="color:blue;">,</mark> <mark style="color:blue;"></mark>_<mark style="color:blue;">n\_observations=None</mark>_<mark style="color:blue;">)</mark>

**Store data for dependency analysis.**

Data takes a 2-dimensional array representing measurements in two dimensions: processes (p) and observations (s). The order of the dimensions in the provided array are specified using a two-character string, either '_ps_' for an array with realisations over (dim 1) processes, and (dim 2) observations in time, or '_sp_', denoting an array over (dim 1) observations in time, and (dim 2) processes.

### Example

```python
# Initialise empty data object
data = Data()

# Load a prefilled financial dataset
data_forex = Data().load_dataset(forex)

# Create data objects with data of various sizes
d = np.arange(3000).reshape((3, 1000))  # 3 procs.,
data_2 = Data(d, dim_order='ps')        # 1000 observations

# Overwrite data in existing object with random data
d = np.arange(5000)
data_2.set_data(data_new, 's')
```

***

<table><thead><tr><th width="190">Parameters:</th><th>Description</th></tr></thead><tbody><tr><td></td><td><ul><li><strong>data</strong> (<em>array_like, optional</em>) – 2-dimensional array with raw data, default=None.</li><li><strong>dim_order</strong> (<a href="https://docs.python.org/3/library/stdtypes.html#str"><em>str</em></a><em>, optional</em>) – Order of dimensions, accepts two combinations of the characters ‘p’, and ‘s’ for processes and observations, default=ps’ (process along the first axis and observation along the second axis).</li><li><strong>detrend</strong> (<a href="https://docs.python.org/3/library/functions.html#bool"><em>bool</em></a><em>, optional</em>) - If True, detrend the dataset along the time axis before normalising (if enabled), default=True.</li><li><strong>normalise</strong> (<a href="https://docs.python.org/3/library/functions.html#bool"><em>bool</em></a><em>, optional</em>) – If True, z-score normalise the dataset along the time axis before computing SPIs, default=True.</li><li><strong>name</strong> (<a href="https://docs.python.org/3/library/stdtypes.html#str"><em>str</em></a><em>, optional</em>) – Name of the dataset</li><li><strong>procnames</strong> (<a href="https://docs.python.org/3/library/stdtypes.html#list"><em>list</em></a><em>, optional</em>) – List of process names with length the number of processes, default=None.</li><li><strong>n_processes</strong> (<a href="https://docs.python.org/3/library/functions.html#int"><em>int</em></a><em>, optional</em>) – Truncates data to this many processes, default=None.</li><li><strong>n_observations</strong> (<a href="https://docs.python.org/3/library/functions.html#int"><em>int</em></a><em>, optional</em>) – Truncates data to this many observations, default=None.</li></ul></td></tr></tbody></table>

> `__init__`(_data=None_, _dim\_order='ps'_, _detrend=False,_ _normalise=True_, _name=None_, _procnames=None_, _n\_processes=None_, _n\_observations=None_)

## Methods

| Method                                                                                                                                                          | Description                                                      |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| [`__init__`](https://pyspi-toolkit.readthedocs.io/en/latest/generated/pyspi.data.Data.html#pyspi.data.Data.__init__)(\[data, dim\_order, normalise, name, ...]) | -                                                                |
| `add_process`(proc\[, verbose])                                                                                                                                 | Appends a univariate process to an existing dataset.             |
| `convert_to_numpy`(data)                                                                                                                                        | Converts other data instances to default numpy format.           |
| `remove_process`(procs)                                                                                                                                         | Remove a univariate process from an existing dataset.            |
| `set_data`(data\[, dim\_order, name, ...])                                                                                                                      | Overwrite data in an existing Calculator instance with new data. |
| `to_numpy`(\[realisation, squeeze])                                                                                                                             | Return the dataset as a numpy array.                             |

## Attributes

| Attribute   | Description              |
| ----------- | ------------------------ |
| `name`      | Name of the data object. |
| `procnames` | List of process names.   |
|             |                          |

