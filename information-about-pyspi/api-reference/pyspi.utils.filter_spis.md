---
coverY: 0
---

# pyspi.utils.filter\_spis

## <mark style="color:purple;">**filter\_spis**</mark>(_keywords:_ [_list_](https://docs.python.org/3/library/stdtypes.html#list)_,_ _output\_name:_ [_str_](https://docs.python.org/3/library/stdtypes.html#str) _= None_, _configfile:_ [_str_](https://docs.python.org/3/library/stdtypes.html#str) _= None_)

Filter a _pyspi_ config YAML file using a list of keywords and return a new YAML file containing the reduced set of SPIs in the current working directory.&#x20;

## Example

```python
from pyspi.utils import filter_spis

# using a user-specified source config file
filter_spis(["linear"], "linear_spis", "./myconfig.yaml")

# using the default config file in the pyspi dir
filter_spis(["linear"], "linear_spis")

# using a randomly generated name and default config file
filter_spis(["linear"]")
```

## Parameters:

| Parameter     | Type                                                                     | Description                                                                                                                                          |
| ------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `keywords`    | [_list_](https://docs.python.org/3/library/stdtypes.html#list)           | List of keywords used to filter SPIs e.g., linear, nonlinear, signed.                                                                                |
| `output_name` | [_str_](https://docs.python.org/3/library/stdtypes.html#str)_, optional_ | Name of the filtered YAML file to be saved in the current working directory. If no name is provided, a randomly generated name will be used.         |
| `configfile`  | [_str_](https://docs.python.org/3/library/stdtypes.html#str)_, optional_ | Location of the source config file from which to filter SPIs. If no source file is provided, the config.yaml in the _pyspi_ directory will be used.  |

## Returns

`Nothing` - A new file is created in the current working directory.&#x20;

## Raises:

* **ValueError**: Raised in three scenarios:
  * If the `keywords` parameter is not a list.
  * If any of the provided keywords is not a string.
  * If no SPIs match the specified keywords.
* **FileNotFoundError**: Triggered if the specified `configfile` cannot be found or if the default `config.yaml` is not present in the expected directory.
* **OError**: Occurs if there's an error reading the specified YAML file.
