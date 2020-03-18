---
description: >-
  Before we can install VANTAGE6, some preliminary steps should be taken. In
  this section we make some suggestions on tools to use, however in most cases
  other solutions will work too.
---

# Preliminaries

## What is in the Package

VANTAGE6 is completely written in Python. It requires at least Python version 3.5+, as f-string are used. The package consist of three components: client, server and node. The client provides a \(Python\) interface to the server \(e.g. creating a new computation task\). It also takes care of end-to-end encryption, encoding and authentication. Internally the client is also used by the node to communicate to the server. The server keeps track of permissions, computation tasks and results. Finally, the node is the data-station which execute computation tasks and has access to the local data.

{% hint style="warning" %}
In case you do not want to use Python as client, you either need to write this yourself, our look for an already existing implementation. An R client is also available.
{% endhint %}

## Basic Requirements

We made the requirements as simple as possible. All components work on Windows 10, MacOS and Linux \(Even works on Raspberry Pi\). They do require **Python 3.5+** and a **working internet connection**. 

{% hint style="info" %}
When VANTAGE6 is installed, it contains all three components. 
{% endhint %}

