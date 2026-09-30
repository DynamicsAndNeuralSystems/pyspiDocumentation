---
cover: >-
  https://images.unsplash.com/photo-1676311522524-fa7c0bffd644?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHx0cm91Ymxlc2hvb3Rpbmd8ZW58MHx8fHwxNzEyMjczODIwfDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Troubleshooting

Welcome to the troubleshooting page for _pyspi_. This section is dedicated to helping you resolve common issues and challenges you might encounter while installing our package. Here, we provide detailed solutions and guidance for a variety of scenarios that new users may face. Our aim is to make your experience with _pyspi_ as smooth and efficient as possible.

Please note that this document is continually updated with new information and solutions. If you encounter a problem that is not covered in this documentation, we encourage you to raise an issue on our [GitHub repository](https://github.com/DynamicsAndNeuralSystems/pyspi). Your feedback is invaluable to us, and we will gladly incorporate new resolutions and updates into this documentation to assist others. We are committed to maintaining a comprehensive and user-friendly troubleshooting guide.

***

## Operating System-Specific Troubleshooting

We have sorted the troubleshooting guide by operating system. Select your operating system to receive relevant troubleshooting advice.

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Windows</strong></td><td></td><td><a href="../../.gitbook/assets/windows_os_icon_light.png">windows_os_icon_light.png</a></td><td><a href="troubleshooting.md#windows-related-issues">#windows-related-issues</a></td></tr><tr><td></td><td align="center"><strong>Linux</strong></td><td></td><td><a href="../../.gitbook/assets/linux_os_icon_light.png">linux_os_icon_light.png</a></td><td><a href="troubleshooting.md#linux-related-issues">#linux-related-issues</a></td></tr><tr><td></td><td align="center"><strong>MacOS</strong></td><td></td><td><a href="../../.gitbook/assets/mac_os_icon_light.png">mac_os_icon_light.png</a></td><td><a href="troubleshooting.md#macos-related-issues">#macos-related-issues</a></td></tr></tbody></table>

***

## Windows Related Issues

***

{% hint style="danger" %}
## Unable to locate Java Paths
{% endhint %}

{% hint style="success" %}
If you are using _Windows_ and you get an error that _pyspi_ cannot locate your system's Octave and/or Java paths, you can manually set these paths prior to importing _pyspi_:

```python
import os
pathToJAVA_HOME = "C:/Program Files/Java/jdk-20" # Change if you use a different Java JDK
os.environ['JAVA_HOME'] = pathToJAVA_HOME
```
{% endhint %}

_Thank you to GitHub user_ [_rmzargar_](https://github.com/rmzargar) _for reporting this issue and providing the solution._

***

{% hint style="danger" %}
## _python setup.py egg\_info_ did not run successfully
{% endhint %}

{% hint style="success" %}
A few users have reported encountering this issue during the installation process (particularly those on Windows OS). This appears to be related to a compatibility conflict between &#x50;_&#x79;thon_ version _3.9.0_ and _sktime_ version _0.8.0_. The current workaround is to re-install pyspi (either in a new conda environment or on your system) using _**python-3.8.0:**_

```bash
$ conda create --name pyspi python=3.8.0
$ conda activate pyspi
# navigate to your pyspi directory
$ pip install .
```
{% endhint %}

***

{% hint style="danger" %}
### Pandas Int64Index error

A few users have noted the following error when instantiating the `Calculator` object:

```python
ImportError: cannot import name 'Int64Index' from 'pandas'
```
{% endhint %}

{% hint style="success" %}
You can fix this error by manually removing numpy and pandas, and manually reinstalling the below versions with the following code:

```python
pip uninstall numpy
pip uninstall pandas
pip install numpy==1.22
pip install pandas==1.3.3
```
{% endhint %}

_Thank you to GitHub user_ [_rmzargar_](https://github.com/rmzargar) _for reporting this issue and providing the solution._

***

## MacOS Related Issues

***

{% hint style="danger" %}
### Java JVM DLL not found

```python
OSError: [Errno 0] JVM DLL not found /Library/Java/JavaVirtualMachines/jdk-19.jdk/Contents/Home/lib/libjli.dylib
```
{% endhint %}

{% hint style="success" %}
This issue is similar to those reported [here](https://stackoverflow.com/questions/71504214/jvm-dll-not-found-but-i-can-clearly-see-the-file) and [here](https://stackoverflow.com/questions/71504214/jvm-dll-not-found-but-i-can-clearly-see-the-file); it can arise from the version of OpenJDK identified as the system default. Some Java versions don't include all of the binary (DLL) files that _pyspi_ looks for. We recommend following this helpful [tutorial](https://blog.bigoodyssey.com/how-to-manage-multiple-java-versions-in-macos-e5421345f6d0) by Chamika Kasun to install AdoptOpenJDK. In a nutshell, here are the steps you should follow:

* Install homebrew if you haven't already

```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

* Install jenv as your Java version manager

```bash
$ brew install jenv
```

* Add jenv to your shell's configuration file (e.g. _.bashrc_ if you use bash, _.zshrc_ if you use zsh)

```bash
$ export PATH="$HOME/.jenv/bin:$PATH"
$ eval "$(jenv init -)"
```

* Source your shell's configuration file:

```bash
$ source ~/.bashrc # If you use bash; OR
$ source ~/.zshrc # If you use zsh
```

* Confirm proper installation of _jenv_

```bash
$ jenv doctor
```

Even if this returns some errors, as long as you see _jenv_ is correctly loaded, you're all set. We recommend using _**AdoptOpenJDK**_ _**version 11**_. Note that AdoptOpenJDK has recently transitioned to Eclipse Adoptium, but in some cases, the Adoptium versions might not resolve specific issues. We suggest downloading an older version of AdoptOpenJDK. Here's how you can proceed:

* Go to the [_AdoptOpenJDK GitHub releases page_](https://github.com/AdoptOpenJDK/openjdk11-binaries/releases).
* Look for the version of AdoptOpenJDK for your OS. For Apple Silicon (M1/M2), the OpenJDK11U-jdk\_x64\_mac\_hotspot\_11.0.9.1\_1.pkg, while not specifically designed for the Apple ARM architecture, has been found to resolve certain issues.&#x20;
* Follow the installation instructions for your specific operating system to install the JDK. Make note of the location in which the JDK was installed.&#x20;

Now you will need to add your _AdoptOpenJDK_ path to your _jenv_ environments. First, you can find where your jdk files are installed with the following command:

```bash
$ /usr/libexec/java_home -V
```

This will list all your installed java JDK versions. Locate the one for _AdoptOpenJDK_ version 11 and paste the path:

```bash
$ jenv add <path_to_adopt_open_jdk_11>
```

Confirm _AdoptOpenJDK_ version 11 was added to _jenv_:

```bash
$ jenv versions
```

You can set _AdoptOpenJDK_ version 11 as your global Java version with the following:

```
$ jenv global <AdoptOpenJDK version>
$ # example:
$ jenv global 11.0
```
{% endhint %}

***

{% hint style="danger" %}
### Pandas Int64Index error

A few users have noted the following error when instantiating the `Calculator` object on MacOS:

```python
ImportError: cannot import name 'Int64Index' from 'pandas'
```
{% endhint %}

{% hint style="success" %}
You can fix this error by manually removing numpy and pandas, and manually reinstalling the below versions with the following code:

```python
pip uninstall numpy
pip uninstall pandas
pip install numpy==1.22
pip install pandas==1.3.3
```
{% endhint %}

***

## Linux Related Issues

***

{% hint style="danger" %}
## Missing pyEDM (Linux)
{% endhint %}

{% hint style="success" %}
Some operating systems/pip versions may be unable to locate pyEDM. In this case, the workaround is to download and install the wheel locally as follows:

1. Download the wheel file (.whl) from the [_pyEDM_ repository](https://pypi.org/project/pyEDM/#files) for version 1.9.3 (one of the two Linux versions depending on whether you are running i686 or x86\_64 architecture).
2. Install the `wheel` package if you don't have it already using `pip install wheel.`
3. Navigate in your terminal to where you've downloaded the wheel file.&#x20;
4. Install the wheel file directly with the command `pip install [file.whl]`. For example, that might look like `pip install pyEDM-1.9.3.0-cp39-cp39-manylinux2010_x86_64.whl`.&#x20;
{% endhint %}

***
