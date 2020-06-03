---
description: 'To dockerize or not to dockerize, that''s the question'
---

# What to install?

Depending on your use case you need to install one or more components in order to work with **vantage6**. In this section we tell you wat to install depending on your use-case.

## Creating computation requests
Creating computation requests can be done by either a researcher or an external application. The **vantage6** server accepts computation requests (tasks) through HTTP requests containing a JSON body. This JSON body contains the algorithm, input and an address where the computation request should be executed. Depending on the algorithm the expected input (and the serialization) is different.

{% hint style="info" %}
The current algorithms that are developed for vantage6 are language specific. I.e. in order for the algorithm to understand input, it needs to be send from a specific language. E.g. The [CoxPH](https://github.com/IKNL/vtg.coxph) model is written in `R` and expects the input to be in [RDS](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/readRDS) format. Therefore the the computation request also needs to be send from `R`.

💡 We are currently working on a solution that resolves this language barrier.
{% endhint %}

## Provide data of researchers or external applications
TODO

## Development of vantage6
TODO

## Development of algorithms for vantage6
TODO

## Host a vantage6 to connect parties
TODO

## General requirements

### Python3

Installation of any of the **vantage6** packages requires Python ≥ 3.6. For installation instructions, see [python.org](https://python.org), [anaconda.com](https://anaconda.com) or use the package manager native to your OS and/or distribution \(e.g. `apt` for debian or Ubuntu, `yum` for fedora, or `yast` for SuSE\).

{% hint style="info" %}
We recommend you install **vantage6** in a new, clean environment.
{% endhint %}

### Docker

Running a node _always_ requires the Docker runtime to be installed \(and running\). Running the server only requires docker when it's run as a dockerized application.

The correct installation of docker can be verified with the following command:

```bash
docker run hello-world
```

{% hint style="info" %}
🐳 Always make sure that Docker is running!

🐳 If this command is not successful, please see [Docker's website](https://www.docker.com/get-started) for installation instructions specific for your platform.
{% endhint %}

## **vantage6**

**vantage6** consists of several components that can be installed and run in several different ways. Which component\(s\) you need, and how you wish to install them, depends on your use case.

There are three \(or four\) components to consider:

1. The **server**
2. A **node**
3. The **client**
4. A Docker **registry**

The server and the node can be _run_ through [Docker](https://www.docker.com) \(preferably\) or _installed_ as regular Python packages/applications. We will cover both options in the following sections.
