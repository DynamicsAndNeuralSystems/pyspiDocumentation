---
description: Example workflow for distributing pyspi calculations on a PBS cluster.
cover: >-
  https://images.unsplash.com/photo-1506399558188-acca6f8cbf41?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxjb21wdXRlciUyMHNlcnZlcnxlbnwwfHx8fDE3MDgzNzc4MTB8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Distributing Calculations

Let's begin by importing the necessary libraries for this simple workflow example:

```python
import pandas as pd
import numpy as np
import os 
import random
import dill
```

You will also need to **download** the following scripts for distributing _pyspi_ jobs across a PBS-type cluster:

* [`distribute_jobs.py`](https://github.com/DynamicsAndNeuralSystems/pyspi-distribute/blob/main/distribute_jobs.py)
* [`pyspi_compute.py`](https://github.com/DynamicsAndNeuralSystems/pyspi-distribute/blob/main/pyspi_compute.py)
* [`template.pbs`](https://github.com/DynamicsAndNeuralSystems/pyspi-distribute/blob/main/template.pbs)

Create a new directory for your workflow (e.g., `pyspi-distribute/`) and copy over the three scripts from above. Also create a folder for your multivariate time series data. Here we will call this folder  `example_data`.  Your file structure should look like the following:

```
📂 pyspi-distribute/
 ┣ 📂example_data/
 ┣ 📜distribute_jobs.py
 ┣ 📜pyspi_compute.py
 ┗ 📜template.pbs
```

## 1. Preparing the MTS data

Save each multivariate time series (MTS) to its own numpy binary file (._npy_). We'll generate three example datasets that are each stored as a separate entry in a dictionary.&#x20;

```python
# generate some example MTS data for this example
random.seed(42)
M = 3 # 3 independent processes
T = 100 # 100 samples/observations per process

# generate a dictionary of 3 MTS
MTS_datasets = {"Dataset_"+str(i) : np.random.randn(M,T) for i in range(3)}

# now save the datasets to separate .npy files
for i, dataset in enumerate(MTS_datasets):
    np.save('example_data/multivariate_time_series_{}.npy'.format(i), dataset)
```

_**Note**_: _By default, each process is z-scored along the time domain, so there is no need to normalise the data beforehand. If you would like to disable this functionality, you can do so by setting `normalise=False` in line 108 of `distribute_jobs.py`._&#x20;

***

## 2. Configuring the YAML file

Now that we have saved our MTS data to separate ._npy_ files, we need to provide a configuration file which specifies the relative location of each `.npy` file (and, optionally, the `name`, `dim_order`, and any relevant `labels`). See [here](../advanced-usage/distributing-calculations-on-a-cluster.md#id-3.-configuring-the-.yaml-file) for more details on how to create a YAML file for distributing pyspi calculations. For this example, we will use the following code to generate the YAML called `sample.yaml:`

```python
# define the YAML file location
yaml_file = "example_data/sample.yaml"

# ps -> rows = processes; columns = time pts.
dim_order = "ps"

# Iterate over the keys and vals of the MTS dictionary
for key, value in MTS_datasets.items():
    # define template string and fill in variables
    yaml_string = "{{file: example_data/{key}.npy, name: {key}, dim_order: {dim_order}, labels: [{key}]}}\n"
    yaml_string_formatted = f"{yaml_string.format(key=key, dim_order=dim_order)}"

    # append line to file
    with open(yaml_file, "a") as f:
        f.write(yaml_string_formatted)
```

This will produce the following YAML file:

{% code title="sample.yaml" %}
```yaml
{file: example_data/Dataset_0.npy, name: Dataset_0, dim_order: ps, labels: [Dataset_0]}
{file: example_data/Dataset_1.npy, name: Dataset_1, dim_order: ps, labels: [Dataset_1]}
{file: example_data/Dataset_2.npy, name: Dataset_2, dim_order: ps, labels: [Dataset_2]}
```
{% endcode %}

Note that here we set the MTS name to "Dataset\_{X}" as well as the labels, but you can use the `labels` argument to include any metadata about a given MTS that you wish.&#x20;

Your file structure prior to submitting the jobs to the PBS cluster should look like the following:

```
📂 pyspi-distribute/
 ┣ 📂example_data/
 ┃ ┣ 📜Dataset_0.npy
 ┃ ┣ 📜Dataset_1.npy
 ┃ ┣ 📜Dataset_2.npy
 ┃ ┗ 📜sample.yaml
 ┣ 📜distribute_jobs.py
 ┣ 📜pyspi_compute.py
 ┗ 📜template.pbs
```

***

## 3. Submitting jobs to the PBS cluster

Now that we've saved our MTS datasets to .npy files and generated the configuration YAML file, we're ready to submit PBS jobs through `pyspi-distribute`. Use the following shell script template as a guide:

```bash
cmd = "python3 distribute_jobs.py --data_dir example_data/ \
    --calc_file_name CALC_FILE_NAME_HERE --compute_file pyspi_compute.py \
    --template_pbs_file template.pbs --sample_yaml example_data/sample.yaml \
    --pbs_notify a --email YOUR_EMAIL_HERE \
    --conda_env YOUR_CONDA_ENV_HERE --queue YOUR_PBS_QUEUE_HERE \
    --walltime_hrs WALLTIME_HOURS_HERE --cpu CPUS_REQUEST_HERE \
    --mem MEMORY_REQUEST_GBS_HERE --table_only"

echo $cmd
$cmd
```

Please replace the following placeholders with your actual values before running this command:

* `CALC_FILE_NAME_HERE`: Name of your calculation file (e.g., `calc.pkl`).
* `YOUR_EMAIL_HERE`: Your email address to receive notifications (e.g., `example@example.com`).
* `YOUR_CONDA_ENV_HERE`: Name of your Conda environment (e.g., `my_conda_env`).
* `YOUR_PBS_QUEUE_HERE`: Name of your PBS queue (e.g., `batch` or `short`).
* `WALLTIME_HOURS_HERE`: Number of hours requested for walltime (e.g., `4`).
* `CPUS_REQUEST_HERE`: Number of CPUs requested (e.g., `8`).
* `MEMORY_REQUEST_GBS_HERE`: Amount of memory requested in GBs (e.g., `32`).

It is recommended you do a trial run with a small example dataset to get a sense of the time/memory requirements for your full dataset before submitting all the jobs. We also include the optional flag:         `--table_only` such that only the SPI results table is saved as opposed to the entire `calculator` object, but you can omit this if you wish to save the whole object.

***

## 4. Accessing results

Once an individual PBS job is completed, you will find a new folder in your data directory (`example_data/`) with the corresponding sample name (e.g., `Dataset_0`) that contains job output information as well as the saved _pyspi_ computation result in the ._pkl_ file. Since we set the `--table_only` flag, we can read in pickle file to get the SPI results table:

```python
# load the results table for dataset 0
with open('example_data/Dataset_0/calc.pkl', 'rb') as f:
    Dataset_0_res = dill.load(f)
    
# Print the results for the empirical covariance:
Dataset_0_res['cov_EmpiricalCovariance']
```

***
