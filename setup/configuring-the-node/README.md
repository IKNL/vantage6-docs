---
description: >-
  In this section you will learn how  to configure the node. This can be done
  either through the command line or by creating a YAML configuration file
  yourself.
---

# Node

{% hint style="danger" %}
At this moment the docker image which contains the node needs to be pulled manually: `docker pull harbor.distributedlearning.ai/infrastructure/node`
{% endhint %}

It is assumed that Docker, and the VANTAGE6 Python package have been successfully installed. If not, have a look at previous steps described in [Installation of Docker](../preliminaries/install-docker.md) and [Install Vantage](../install-vantage6.md).

{% hint style="info" %}
A data-provider needs a node for each of the collaboration it participates in.
{% endhint %}

## 💻 Node Commands

In the section above we used the`vnode` command several times. All sub-commands can be found by simply running `vnode` , which will display the following list:

| Command | Description |
| :--- | :--- |
| `vnode new` | Create a new configuration file |
| `vnode list` | List all available configurations \(both system/user\) |
| `vnode files` | List file locations of the node instance |
| `vnode start` | Start a node configuration |
| `vnode attach` | Attach the outl |

## 🆕 \(Re-\)Configuring a Node

Nodes are created from a configuration file. This file can be created through command `vnode new`or creating a YAML file manually. See [this ](../server/server-configuration.md)section on how to do this. 

##  🏎 Starting the Node

Once a configuration is created you can start this instance by using `vnode start` and select the configuration you wish to start. Or in the case you already know the name of the configuration you can use `vnode start --name [config_name]` . It is also possible to use a configuration file that is in a non-default location by using `vnode start --config /path/to/config` . All other options will be overwritten when using this option.

{% hint style="warning" %}
Node configurations are assumed to be in the user-folder. However if you have a configuration stored in the system-folder you should add the flag system: `vnode start --system` .  
{% endhint %}

## ✍ Logging

The easiest way to read the logs from a node is using `vnode attach` and select the node from which you want to read the logs from. 



