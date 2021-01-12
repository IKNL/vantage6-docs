---
description: 'To dockerize or not to dockerize, that''s the question'
---

# What to install?

**vantage6** consists of several components that can be installed and run in several different ways. Which component\(s\) you need, and how you wish to install them, depends on your use case.

## 📧 Creating computation requests **\[client\]**

Creating computation requests can be done by either a researcher or an external application. The **vantage6** server accepts computation requests \(tasks\) through HTTP requests containing a JSON body. This JSON body contains the algorithm, input and an address where the computation request should be executed. Depending on the algorithm the expected input \(and the serialization\) is different.

For both `Python` and `R` there is a client package available. But you can also create your custom interface in your preferred language to communicate to the [API](https://trolltunga.vantage6.ai/apidocs).

{% tabs %}
{% tab title="Python" %}
```python
pip install vantage6-client
```
{% endtab %}

{% tab title="R" %}
```r
devtools::install_github('IKNL/vtg.basic', subdir='src')
```
{% endtab %}
{% endtabs %}

See the section [Running analysis](../usage/running-analyses/) for more info on how to use these clients.

{% hint style="info" %}
The current algorithms that are developed for **vantage6** are language specific. I.e. in order for the algorithm to understand input, it needs to be send from a specific language. E.g. The [CoxPH](https://github.com/IKNL/vtg.coxph) model is written in `R` and expects the input to be in [RDS](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/readRDS) format. Therefore the the computation request also needs to be send from `R`.

💡 We are currently working on a solution that resolves this language barrier.
{% endhint %}

## 💁 Provide data access **\[node\]**

As a data provider for the **vantage6** solution you need to install the node package. There are two flavors, Docker \(preferred\) or non-Docker. In any case, make sure you install the [General requirements](general-requirements.md) first.

## 🚉 Host a vantage6 server **\[server\]**

A \(central\) server allows parties to connect and exchange computation tasks and their results. To do this you need to install the server package. There are, just as in the case of the node, two flavors: Docker \(preferred\) or non-Docker. Make sure you go though the [General requirements](general-requirements.md) before installing the [Docker](dockerized-installation.md) or [non-Docker](non-dockerized-installation.md).

Besides installing the **vantage6** server you might also want to host a Docker registry. This would contain images that can be used for computations in the **vantage6** solution. See [Docker registry](docker-registry.md) on how to setup this.

## 👷 Development of vantage6

Make sure you installed the [General requirements](general-requirements.md). As a vantage6-developer you should clone our [master repository](https://github.com/iknl/vantage6-master) \(and all its sub-modules\).

```text
git clone https://github.com/IKNL/vantage6-master
```

Then install the sub-modules \(packages\) as [editable](https://pip.pypa.io/en/stable/reference/pip_install/#editable-installs):

```text
pip install -e vantage6-master/vantage6-common
pip install -e vantage6-master/vantage6-client
pip install -e vantage6-master/vantage6
pip install -e vantage6-master/vantage6-node
pip install -e vantage6-master/vantage6-server
```

## 🚧 Development of algorithms for vantage6

Section to be created in the future. Please contact us on [Discord ](https://discord.gg/WxcrzXp)if you want to develop an algorithm!

