---
description: An overview of the vantage6 infrastructure and its components
---

# Architecture

## Overview

Vantage6 uses both a client-server and peer-to-peer model. In the figure below the **client** can pose a question to the **server,** the question is then delivered as an algorithm to the node. When the algorithm completes, the results are sent back to the client via the server. An algorithm can communicate directly with other algorithms that run on other nodes if required.

![Vantage6 has a client server architecture. (A) The Client is used by the researcher to create computation requests. It is also used to manage users, organizations and collaborations. (B) The Server contains users, organizations, collaborations, tasks and their results. (C) The Node has access to data and handles computation requests from the server. ](<../.gitbook/assets/Artboard 1@4x (1).png>)

The server is in charge of processing the tasks as well as of handling administrative functions such as authentication and authorization. Conceptually, vantage6 consists of the following parts:

* A (central) **server** that coordinates communication with clients and nodes
* One or more **node(s)** that have access to data and execute algorithms
* **Organizations** that are interested in collaborating&#x20;
* **Users** (i.e. researchers or other applications) that request computations from the nodes
* A **Docker registry** that functions as database of algorithms&#x20;

## Components

In this section we explain each of the individual components that are part of the vantage6 network.

### Server

{% hint style="warning" %}
Here, when we refer to the server, this includes not only the vantage6-server, but also other components that the vantage6-server uses.&#x20;
{% endhint %}

The server is responsible for coordinating all communication in the vantage6 network. It consists of several components:&#x20;

* vantage6-server
* Docker registry
* VPN server (Optionally)

The **vantage6-server** contains the users, organizations, collaborations, tasks and their results. It handles authentication and authorization to the system and is the central point of contact for clients and nodes. The **Docker registry** contains algorithms which can be used by clients to request a computation. The **VPN server** is required if algorithms need to be able to engage in peer-to-peer communication.&#x20;

### Node

The node is responsible for executing the algorithms on the **local data**. It protects the data by allowing only specified algorithms to be executed after verifying their origin. The **vantage6-node** is responsible for picking up the task, executing the algorithm and sending the results back to the server. The node needs access to local data. This data can either be a file (e.g. csv) or a service (e.g. a database).

### Client

The client provides an interface to the server. This allows users and applications to create tasks and retrieve their results. The client also enables you to manage entities at the server (i.e. creating users, organizations and collaborations). Note that the client can directly interact with the server through the API or using one of our client libraries (e.g the [python client](broken-reference)).
