---
description: A (central) server allows parties to connect and exchange data.
---

# Server

To install the **vantage6-server** make sure you have met the [requirements](../what-to-install/#node-and-server). Then install the latest version:

```
pip install vantage6
```

#### Optional components

There are two optional components that you can set up apart from the vantage6-server itself.

A docker registry can be used to store algorithms but it is also possible to use [Docker hub](https://hub.docker.com) for this. For instructions on how to install your own Docker registry see [docker-registry.md](docker-registry.md "mention").&#x20;

If you want to enable algorithm containers that are running on different nodes, to directly communicate with one another, you require a VPN server. Refer to [eduvpn.md](eduvpn.md "mention") on how to install the VPN server.

{% hint style="info" %}
Currently it is not possible to communicate to nodes via their public IPs
{% endhint %}

