---
description: 'To dockerize or not to dockerize, that''s the question'
---

# What to install?

**Vantage6** consists of several components that can be installed and run in several different ways. Which component\(s\) you need, and how you wish to install them, depends on your use case.

There are three \(or four\) components to consider:

1. The **server**
2. A **node**
3. The **client**
4. A Docker **registry**

The server and the node can be installed either as regular Python packages/applications or they can be _run_ through [docker](https://www.docker.com). These options will be further detailed in the following sections. However, there are a few general requirements, which are detailed below.

## General requirements

### Python3

Installation of any of the **vantage6** packages requires Python ≥ 3.6. For installation instructions see [python.org](https://python.org), [anaconda.com](https://anaconda.com) or use the package manager native to your OS and/or distribution \(e.g. `apt` for debian or Ubuntu, `yum` for fedora, or `yast` for SuSE\).

### Docker

Running a node _always_ requires the Docker runtime to be installed \(and running\). Running the server only requires docker when it's run as a dockerized application. 

The correct installation of docker can be verified with the following command:

```bash
docker run hello-world
```

If this command is not succesful, please see [docker.com](https://www.docker.com/get-started) for installation instructions specific for your platform.

{% hint style="warning" %}
When using Windows, make sure you [share ](https://docs.docker.com/docker-for-windows/)you're drives with Docker.
{% endhint %}

