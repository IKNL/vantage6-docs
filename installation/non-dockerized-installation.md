---
description: Installation using the separate Python packages
---

# Non-dockerized installation

Running the server or node non-dockerized can be useful for development. The applications are made available as separate Python packages on [PyPI](https://pypi.org) and can be installed through `pip`.

{% hint style="info" %}
Depending on your local setup you might need to substitute `pip` with `pip3`.
{% endhint %}

## Server

The server can be installed through Python's pip command:

```text
pip install vantage6-server
```

After installation, the command `vserver-local` should be available from the command prompt.  This can be verified by running the following command:

```text
vserver-local --help
```

Configuring and running the server is explained in the section [Running the server \(dockerized\)](../usage/running-the-server/). Just make sure to substitute the command `vserver` with `vserver-local`!

## Node

The node can be installed through Python's pip command:

```text
pip install vantage6-node
```

After installation, the command `vnode` should be available from the command prompt. This can be verified by running the following command:

```text
vnode-local --help
```

Configuring and running nodes is explained in the section [Running the Node \(dockerized\)](../usage/running-the-node/). Just make sure to substitute the command `vnode` with `vnode-local`!

