---
description: >-
  This page is intended for users to install Python and an add environment to
  which VANTAGE6 can be installed.
---

# Python

Python can be installed in various ways. it is recommended to use environments. [Python 3.3+](https://www.python.org/downloads/) comes [venv](https://docs.python.org/3/library/venv.html), but you can also use other tools like [miniconda](https://docs.conda.io/en/latest/miniconda.html). In case you use miniconda, you do not need to install python separately as it comes with it. We only explain the case where you use Python venv or in case you use miniconda but other environment managers should also work.

## Setup using venv

Install [Python 3.3+](https://www.python.org/downloads/) from their website. Once installed, create a virtual environment:

{% tabs %}
{% tab title="Bash" %}
```bash
python3 -m venv /path/to/new/virtual/environment
```
{% endtab %}

{% tab title="Windows" %}
```
c:\>c:\Python35\python -m venv c:\path\to\myenv
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
In the case of Windows, you need to specify the full path to the python executable. To avoid this you could also add the folder which contains the python executable to the environment variable `PATH`. 
{% endhint %}

To activate an environment:

{% tabs %}
{% tab title="Bash" %}
```bash
source <venv>/bin/activate
```
{% endtab %}

{% tab title="Windows" %}
```
C:\> <venv>\Scripts\activate.bat
```
{% endtab %}
{% endtabs %}

## Setup using miniconda

Install miniconda from their website. Once installed, create a virtual environment:

```text
conda create -n environment python=3.7
```

{% hint style="info" %}
On windows, during installation you have to option to add the commands to the `PATH` variable. If you do not do this, you need to execute the commands above in the Anaconda Prompt \(which should already be installed\)
{% endhint %}

To activate an environment:

{% tabs %}
{% tab title="Bash" %}
```bash
conda activate environment
```
{% endtab %}

{% tab title="Windows" %}
```
activate environment
```
{% endtab %}
{% endtabs %}





