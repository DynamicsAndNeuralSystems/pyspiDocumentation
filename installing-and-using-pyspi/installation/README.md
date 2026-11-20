---
description: Steps to get pyspi up and running on your system.
cover: >-
  https://images.unsplash.com/photo-1614189313880-13b09111b4de?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw0fHxpbnN0YWxsYXRpb258ZW58MHx8fHwxNzE0MzMxNTI0fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Installation

_pyspi_ requires **Python 3.11 or newer** (it is tested on 3.11, 3.12 and 3.13).
It is pure Python and needs no Java runtime or other external software.

## 1. Create an environment

{% hint style="success" %}
## Recommended: install _pyspi_ in its own environment
{% endhint %}

_pyspi_ depends on a large scientific stack (numpy, scipy, scikit-learn, statsmodels, mne and others), so installing it into a dedicated environment keeps its dependencies from conflicting with your other projects.
Any of the following works.

With the standard library's `venv`:

```bash
python3.12 -m venv .venv
source .venv/bin/activate        # on Windows: .venv\Scripts\activate
```

With [_uv_](https://docs.astral.sh/uv/), which can also download a suitable Python for you:

```bash
uv venv --python 3.12
source .venv/bin/activate
```

With [_conda_](https://docs.conda.io/projects/conda/en/latest/index.html):

```bash
conda create -n pyspi python=3.12
conda activate pyspi
```

## <mark style="color:green;">2. Install with</mark> <mark style="color:green;"></mark>_<mark style="color:green;">pip</mark>_

***

Inside the environment, install _pyspi_ from PyPI:

```bash
pip install pyspi        # or: uv pip install pyspi
```

Check that you have version 3 or later:

```bash
python -c "from importlib.metadata import version; print(version('pyspi'))"
```

If this prints a 2.x version, your Python is older than 3.11 and pip has fallen back to the last release that supported it; see [Troubleshooting](troubleshooting.md).

Two optional extras are available: `pip install "pyspi[tsfile]"` adds support for loading sktime/aeon `.ts` files, and `pip install "pyspi[bench]"` installs what the compute-cost benchmark suite in the repository's `bench/` folder needs.

***

## Alternative Options

For users who are encountering issues related to the installation of _pyspi_, we offer a troubleshooting guide, as well as alternative installation options (from source, or with Docker). Click a card below for alternative options:

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Alternative Installation Options</strong></td><td></td><td><a href="../../.gitbook/assets/alt_install_light.png">alt_install_light.png</a></td><td><a href="alternative-installation-options.md">alternative-installation-options.md</a></td></tr><tr><td></td><td align="center"><strong>Troubleshooting</strong></td><td></td><td><a href="../../.gitbook/assets/troubleshooting_vector_icon (1).png">troubleshooting_vector_icon (1).png</a></td><td><a href="troubleshooting.md">troubleshooting.md</a></td></tr></tbody></table>

***
