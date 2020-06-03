---
description: 'To dockerize or not to dockerize, that''s the question'
---

# What to install?
**vantage6** consists of several components that can be installed and run in several different ways. Which component\(s\) you need, and how you wish to install them, depends on your use case.

Depending on your use case(s) you need to install one or more components in order to work with **vantage6**. In this section we tell you wat to install. 

## 📧 Creating computation requests **[client]**

Creating computation requests can be done by either a researcher or an external application. The **vantage6** server accepts computation requests \(tasks\) through HTTP requests containing a JSON body. This JSON body contains the algorithm, input and an address where the computation request should be executed. Depending on the algorithm the expected input \(and the serialization\) is different.

For both `Python` and `R` there is a client package available. But you can also create your custom interface in your prefered langauge to comminucate to the [API](https://trolltunga.vantage6.ai/apidocs). 

{% tabs %}
{% tab title="R" %}
```R
devtools::install_github('mellesies/vtg.basic', subdir='src')
```
{% endtab %}
{% tab title="R" %}
```Python
pip install vantage6-client
```
{% endtab %}
{% endtabs %}

See the section [Running analysis](/usage/running-analyses/README.md) for more info on how to use these clients.

{% hint style="info" %}
The current algorithms that are developed for vantage6 are language specific. I.e. in order for the algorithm to understand input, it needs to be send from a specific language. E.g. The [CoxPH](https://github.com/IKNL/vtg.coxph) model is written in `R` and expects the input to be in [RDS](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/readRDS) format. Therefore the the computation request also needs to be send from `R`.

💡 We are currently working on a solution that resolves this language barrier.
{% endhint %}

## 💁 Provide data access **[node]**

As a data provider for the **vantage6** solution you need to install the node package. There are two flavors, Docker (prefered) or a non-dockerized version. Make sure you install the [General requirements](general-requirements.md) first before install the [Docker](dockerized-installation.md) or [non-Docker](non-dockerized-installation.md) version of **vantage6**.

## 🚉 Host a vantage6 server **[server]**

A a (central) server allows parties to connect and exchange computation tasks and their results. To do this you need to install the server package. There are, just as in the case of the node, two flavors: Docker (prefered) or non-Docker. Make sure you go though the [General requirements](general-requirements.md) before installing the [Docker](dockerized-installation.md) or [non-Docker](non-dockerized-installation.md).

Besides installing the **vantage6** server you might also want to host a Docker registry. This would contain images that can be used for computations in the **vantage6** solution. See [Docker registry](docker-registry.md) on how to setup this. 


## 👷 Development of vantage6 
TODO

## 🚧 Development of algorithms for vantage6 
TODO