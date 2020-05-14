---
description: 'Installation using the prebuilt, dockerized components'
---

# Dockerized installation

Running the dockerized node and/or server is the preferred way of using  **vantage6**. It requires that the Docker runtime is installed \(and running\) and that the **vantage6** command line interface \(CLI\) is available.

{% hint style="info" %}
See [What to install?](preliminaries.md#docker) for information on how to verify Docker is installed and running properly
{% endhint %}

#### Installing the Command Line Interface

We have provided a command line interface \(CLI\) that facilitates pulling \(i.e., downloading\) and interacting with the server and node images from the docker registry. This CLI can be installed through Python's `pip` command:

```bash
pip install vantage6
```

{% hint style="info" %}
Depending on your local setup you might need to substitute `pip` with `pip3`.
{% endhint %}

After installation, the commands `vnode` and `vserver` should be available in the command prompt. The following commands should run without error \(and provide documentation on how to use them\).

{% tabs %}
{% tab title="Node" %}
```bash
vnode --help
```
{% endtab %}

{% tab title="Server" %}
```bash
vserver --help
```
{% endtab %}
{% endtabs %}

For information on how to use the `vnode` and `vserver` commands, see the sections [Server \(dockerized\)](../usage/running-the-server/) and [Node \(dockerized\)](../usage/running-the-node/).

