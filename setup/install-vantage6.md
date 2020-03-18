---
description: >-
  This guide will show you how to install the VANTAGE6 package. This step is
  required for using both the (python-)client, node and server.
---

# Install VANTAGE6

## 🐍 Python

We recommend using the latest version of Python 3. VANTAGE6 supports Python 3.5 or newer. Older versions of python will **not**  work due to the use of [f-strings](https://docs.python.org/3.6/reference/lexical_analysis.html#f-strings). 

For more details on how to install Python, please read [this](preliminaries/install-python.md) section.

## 🤓 Install 

The latest release can be found in the [pypi repository](https://pypi.org/project/vantage/). VANTAGE6 can be installed by executing the following command in your python environment:

```bash
pip install vantage
```

{% hint style="danger" %}
The pypi-repository will be updated soon to promote the new name for the project
{% endhint %}

## 🖥 Install Development version 

An alternative way of installing VANTAGE6 is to clone our GitHub repository and install it from there. This enables the user to use chose the specific branch in order to make some adjustments to the source code.

```bash
git clone https://github.com/IKNL/ppDLI.git
cd ppDLI
git checkout DEV # optional
pip install -e .
```

{% hint style="danger" %}
pip install the in the folder `~/.local/bin`which is not part of the PATH.  You can use`~/.local/bin/vserver` or you can add `~/.local/bin/` to your 
{% endhint %}

## Install Node without Python

{% hint style="danger" %}
Only use this if you know what your are doing.
{% endhint %}

Since the Node runs in a Docker container it is possible to run a node without installing Python. See [this ](configuring-the-node/docker-only-install.md)page on how to do this.

