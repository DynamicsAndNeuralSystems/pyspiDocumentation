---
cover: >-
  https://images.unsplash.com/photo-1676311522524-fa7c0bffd644?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHx0cm91Ymxlc2hvb3Rpbmd8ZW58MHx8fHwxNzEyMjczODIwfDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Troubleshooting

This page collects solutions to common problems when installing and first running _pyspi_.
If you encounter a problem that is not covered here, please raise an issue on our [GitHub repository](https://github.com/DynamicsAndNeuralSystems/pyspi/issues), including your operating system, Python version and the full error message.

***

## Installation

***

{% hint style="danger" %}
### pip installs pyspi 2.x, or finds no matching version
{% endhint %}

{% hint style="success" %}
_pyspi_ 3 requires Python 3.11 or newer.
On an older Python, pip either reports that no matching distribution was found or silently installs the last 2.x release, which still needs Java.
Check your Python version and the installed _pyspi_ version:

```bash
python --version
python -c "from importlib.metadata import version; print(version('pyspi'))"
```

If either is too old, create a new environment with Python 3.11 or newer (see [Installation](README.md)) and install _pyspi_ there.
{% endhint %}

***

{% hint style="danger" %}
### Errors mentioning Java, JPype, the JVM or infodynamics.jar
{% endhint %}

{% hint style="success" %}
_pyspi_ 3 does not use Java.
An error about `JAVA_HOME`, `jpype`, a missing JVM DLL or `infodynamics.jar` means that _pyspi_ 2.x is installed in the environment you are running.
Install _pyspi_ 3 in a fresh environment with Python 3.11 or newer (see [Installation](README.md)).
The [changelog](https://github.com/DynamicsAndNeuralSystems/pyspi/blob/main/CHANGELOG.md) lists what changed from 2.x, including the `Calculator` API and SPI names.
{% endhint %}

***

{% hint style="danger" %}
### Building dtaidistance fails on Linux ARM (aarch64)
{% endhint %}

{% hint style="success" %}
The `dtaidistance` dependency publishes binary wheels for Linux x86\_64, macOS and Windows, but not for Linux on ARM (aarch64), where pip builds it from source.
Install a C compiler and the Python development headers first (for example `sudo apt install build-essential python3-dev` on Debian or Ubuntu), then rerun `pip install pyspi`.
{% endhint %}

***

{% hint style="danger" %}
### Windows
{% endhint %}

{% hint style="success" %}
_pyspi_ is tested on Linux and macOS.
Its dependencies publish Windows wheels, so `pip install pyspi` is expected to work, but Windows is not part of the test suite.
If you hit a Windows-specific problem, running _pyspi_ under the [Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/install) is the most reliable workaround; please also report it as an issue.
{% endhint %}

***

## First run

***

{% hint style="danger" %}
### Some SPIs return NaN
{% endhint %}

{% hint style="success" %}
Every value in `calc.table` is a real, finite number or NaN.
A NaN can mean the statistic is undefined for that input, that the input was rejected (for example a constant series, or one containing NaN or Inf), or that the computation failed.
`calc.quality` gives one code per cell (from `pyspi.Quality`) saying which, and `calc.errors` gives the error message for every SPI that failed.
For example, the Kraskov (KSG) estimators refuse series with repeated values, so quantised data return NaN for those SPIs.
{% endhint %}

***

{% hint style="danger" %}
### Parallel computation is slow on macOS
{% endhint %}

{% hint style="success" %}
With `n_jobs > 1`, _pyspi_ pins each worker's BLAS and OpenMP thread pools to one thread to avoid oversubscription.
The Accelerate BLAS used on macOS cannot be pinned this way, so on macOS prefer `n_jobs=1` for a single dataset, and parallelise across datasets instead.
{% endhint %}

***
