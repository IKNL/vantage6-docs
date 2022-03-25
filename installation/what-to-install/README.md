# Requirements

**vantage6** consists of several [components ](../../about-background/introduction.md#components)that can be installed. Which component(s) you need depends on your use case. Also the requirements differ per component.&#x20;

## Client

You can interact with the server via the API. You can explore the server API on **https://\<serverdomain>/apidocs** (e.g. [https://petronas.vantage6.ai/apidocs](https://petronas.vantage6.ai/apidocs) for Petronas).

You can use any language to interact with the server as long as it supports HTTP requests. For Python and R we have written wrappers to simplify the interaction with the server: see [client.md](../client.md "mention")for more details on how to install these.

{% hint style="danger" %}
Depending on your algorithm it _may_ be required to use a specific language to retrieve the results. This could happen when the output of an algorithm contains a language specific datatype and or serialization. &#x20;

E.g. when the algorithm is written in R and the output is written back in RDS (=specific to R) you would also need R to read the final input.&#x20;

**Please consult the developer of your algorithm if this is the case.**
{% endhint %}

## Node & Server

The (minimal) requirements of the node and server are similar. Note that not all of these are hard requirements: it could well be that it also works on other hardware, operating systems, versions of Python etc.

**Hardware**

* x86 CPU architecture + virtualization enabled
* 1 GB memory
* 50GB+ storage
* Stable and fast (1 Mbps+ internet connection)
* Public IP address

**Software**

* Operating system
  * Ubuntu 18.04+ or
  * MacOS Big Sur+
  * Windows 10
* [Python 3.7.x](python.md)
* [Docker engine](docker.md)

{% hint style="danger" %}
The hardware requirements of the node also depend on the algorithms that the node will run. For example, you need a lot less compute power for a descriptive statistical algorithm than for a machine learning model.
{% endhint %}
