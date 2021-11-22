---
description: In this section we will explain how to configure and manage a node.
---

# Running the Node

It is assumed that Docker, and the [vantage6](https://pypi.org/project/vantage6/) Python package have been successfully installed. If not, have a look at previous steps described in [What to install?](../../installation/what-to-install.md) and [Dockerized installation](../../installation/dockerized-installation.md).

{% hint style="info" %}
An organisation runs a node for each of the collaborations it participates in.
{% endhint %}

## 🆕 (Re-)Configuring a node

After having a server up and running, we need to create and configure the nodes.

Similar to the server, nodes are created from a configuration file. This file can be generated through the command `vnode new`or manually from a YAML file manually, as explained [in this section. ](configuration.md)

## 🔑 Creating a private key

If you're running _any_ node in a collaboration that requires end-to-end encryption, you'll need to setup a private key _**for your organisation**_. This can be done through the command `vnode create-private-key`.

Running this command without any arguments, will perform the following steps:

1. prompt for a node configuration to use
2. prompt for a username/password
3. retrieve the name of the organisation from the server
4. create a public/private key pair
5. update the node's configuration to point to the (new) private key
6. upload the public key to the server

If a key already exists (in step 4), the existing key is reused. This way, it's easy to configure multiple nodes to use the same key. All nodes within an organisation should use the same private key! Otherwise at least one of the nodes will not be able to process its tasks!

{% hint style="danger" %}
All nodes within an organisation should use the same private key! Otherwise at least one of the nodes will not be able to process its tasks!
{% endhint %}

## 🏎 Starting the Node

Once a configuration is created you can start this instance by using `vnode start` and select the configuration you wish to start. Or in the case you already know the name of the configuration you can use `vnode start --name [config_name]` . It is also possible to use a configuration file that is in a non-default location by using `vnode start --config /path/to/config` . All other options will be overwritten when using this option.

{% hint style="warning" %}
Node configurations are assumed to be in the user-folder. However if you have a configuration stored in the system-folder you should add the flag system: `vnode start --system` .
{% endhint %}

## ✍ Logging

The easiest way to read the logs from a node is using `vnode attach` and select the node from which you want to read the logs from.

## 💻Node Commands

As a reference, these are the sub-commands available to manage the node(s). These commands can also be found by simply calling `vnode` .

| Command              | Description                                          |
| -------------------- | ---------------------------------------------------- |
| `vnode new`          | Create a new configuration file                      |
| `vnode list`         | List all available configurations (both system/user) |
| `vnode files`        | List file locations of the node instance             |
| `vnode start`        | Start a node configuration                           |
| `vnode attach`       | Connect the console to the node's `stdout`           |
| `create-private-key` | Creates and uploads a new public key                 |
