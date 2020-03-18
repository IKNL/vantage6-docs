---
description: >-
  This guide will show you how to install the VANTAGE6 package. This step is
  required for using both the node and server.
---

# Install VANTAGE6

Installing Vantage has a few requirements, the most important one is python $$\geq$$ 3.5. Older version of python will **not** work due to the use of [f-strings](https://docs.python.org/3.6/reference/lexical_analysis.html#f-strings).  

{% hint style="warning" %}
The current stable release of Debian \(Debian 9, stretch\) only sports Python 3.5 out of the box. It is therefore recommended to use Ubuntu 18 or Debian _testing_ \(Debian 10, buster\) to avoid having to install backports.
{% endhint %}

The latest release can be found in the [pypi repository](https://pypi.org/project/ppDLI/). So VANTAGE6 \(formely ppdli\) can be installed by running:

```bash
pip install ppdli
```

{% hint style="danger" %}
The pypi-repository will be updated soon to promote the new name for the project
{% endhint %}

An alternative way of installing is to clone our repository and install it from there. This allows to use a specific branch and allows you to make changes to the source code.

```bash
git clone https://github.com/IKNL/ppDLI.git
cd ppDLI
git checkout DEV # optional
pip install -e .
```

{% hint style="danger" %}
pip install the commands in the folder `~/.local/bin`which is not part of the PATH.  You can use`~/.local/bin/vserver` or you can add `~/.local/bin/` to your `PATH`
{% endhint %}

