---
description: A guide to get started with distributing pyspi jobs across a PBS-type cluster.
cover: >-
  https://images.unsplash.com/photo-1558494949-ef010cbdcc31?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxzZXJ2ZXJzfGVufDB8fHx8MTcxMzkyMzE3OHww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Distributing calculations on a cluster

## _pyspi_ distribute scripts

All scripts necessary for distributing _pyspi_ jobs across a PBS-type cluster are readily accessible in the [_**pyspi-distribute**_ ](https://github.com/olivercliff/pyspi-distribute)GitHub repository. Each job will contain one calculator object that is associated with one multivariate time-series (MTS).

The scripts allow the user to specify a directory containing the MTS files, with each MTS sample being stored in a separate binary NumPy ([.npy](https://numpy.org/doc/stable/reference/generated/numpy.save.html)) file. Within this directory, the user needs to also include a YAML configuration file which specifies the relative location of each `.npy` file (and, optionally, the `name`, `dim_order`, and any relevant `labels`).  An R script to automatically populate this configuration file is provided for the user: `create_yaml_for_samples.R`.

## Using _pyspi_ distribute

The following instructions provide a guide to getting started with distributing calculations on a PBS-type cluster. For a more in-depth example of a typical workflow using _pyspi-distribute_ see the walkthrough tutorial [here](../walkthrough-tutorials/distributing-calculations.md).&#x20;

## 1. Initialising the environment

Follow the _pyspi_[ _installation guide_](../../installation/) to install and setup _pyspi_ on your cluster. We recommend installing into a **new conda environment** to ensure there are no clashes in dependencies.&#x20;

## 2. Organising and formatting your data

Organise all of your multivariate time series into a user-specified data directory. If your data is not already in the correct format to be read into _pyspi_, follow the NumPy guide on saving your data as a `.npy` file [here](https://numpy.org/doc/stable/reference/generated/numpy.save.html).&#x20;

Ensure each multivariate time-series sample is stored in a **separate** `.npy` file.&#x20;

## 3. Configuring the `.yaml` file

Either manually create a `sample.yaml` file or automatically populate it using `create_yaml_for_samples.R`. Ensure that the `.yaml` file is located in the recently created user-specified data directory, along with your MTS samples.

An example of a typical `sample.yaml` file for two MTS samples `sample1.npy` and `sample2.npy` is provided below:

```yaml
- {file: ./database/sample1.npy, name: sample1, dim_order: sp, labels: [synthetic,noise] }
- {file: ./database/sample2.npy, name: sample2, dim_order: sp, labels: [synthetic,noise] }
```

Here, the optional parameters: `name` (str), `dim_order` (str) and `labels` (list of str) are specified alongside the file path of the sample.

<details>

<summary>Automatically generate a sample YAML file (optional)</summary>

If you have many samples, you may wish to automatically populate your sample YAML configuration file. We have provided the R script `create_yaml_for_samples.R` to accomplish this. This script can be run on the command line with the following arguments:

* `--data_dir`: \[_Required_] Data directory in which all samples' MTS NumPy files are stored (e.g. `database/`)
* `--sample_metadata`: \[_Optional_] Path to CSV file containing sample metadata. If supplied, the identifying variable for each sample MTS must be `sampleID`.
* `--label_vars`: \[_Optional_] Variable(s) to include in the YAML file from the metadata.
* `--overwrite`: \[_Optional flag_] If included, `sample.yaml` will be overwritten if if already exists.

</details>

## 4. Submit the _pyspi_ jobs to the PBS cluster

**Activate** your conda environment in the location where pyspi is installed using: `conda activate pyspi`. Then, **submit** the jobs from the command line using **`distribute_jobs.py`** (see below for usage).&#x20;

The script, `distribute_jobs.py` works by taking in a data directory (where your MTS samples and sample.yaml are located), iterating over each MTS NumPy sample, and submitting a separate PBS job for each sample. The pbs file is automatically generated from this script and submitted via `qsub`.&#x20;

The `distribute_jobs.py` script includes several command-line options for user configuration, all of which are optional:

<table><thead><tr><th width="274">Option</th><th>Description</th></tr></thead><tbody><tr><td><code>--data_dir</code></td><td>Data directory in which all samples' MTS NumPy files are stored. If no path is supplied, the default is <code>./database/</code> from the directory in which <code>distribute_jobs.py</code> is located </td></tr><tr><td><code>--compute_file</code></td><td>The file path for python script that actually runs pyspi. Default is <a href="https://github.com/olivercliff/pyspi-distribute/blob/main/pyspi_compute.py">pyspi_compute.py</a> in the directory where this script is located.</td></tr><tr><td><code>--sample_yaml</code></td><td>The file path to the sample YAML configuration file. The default is <code>./database/sample.yaml</code>.</td></tr><tr><td><code>--pyspi_config</code></td><td>If desired, the file path to a user-generated YAML configuration file specifying a subset of SPIs to compute.</td></tr><tr><td><code>--walltime_hrs</code></td><td>Maximum wall-time allowed for a given job, in hours. The default is <em><code>24</code></em>.</td></tr><tr><td><code>--overwrite_pkl</code></td><td>Including this flag means that existing <em>pyspi</em> results for a given sample will be overwritten.</td></tr><tr><td><code>--pbs-notify</code></td><td>When pbs should email user; a=abort, b=begin, e=end. The default is none.</td></tr><tr><td><code>--email</code></td><td>Email address when pbs should notify user.</td></tr></tbody></table>

## 5. Accessing results

The results will be stored in the user-specified data directory under the same name as the numpy files. For example, if you have the file `database/sample1.npy` in your YAML fle, then there will be a new folder called `database/sample1` with a `calc.pkl` file inside that contains the [Calculator](../../../information-about-pyspi/api-reference/pyspi.calculator.calculator.md) object.&#x20;

In order to access the results, you must load the calculator with [dill](https://pypi.org/project/dill/):

```python
import dill

with open('calc.pkl', 'rb') as f:
    calc = dill.load(f)
```

Then you can view the contents as per the standard pyspi documentation, e.g.,&#x20;

```python
calc.table
calc.table['cov_EmpiricalCovariance']
```

***

