---
description: A brief overview of the vantage6 infrastructure.
---

# The infrastructure

Collaboration through federated learning requires an infrastructure. The open source solution [**vantage6**](https://github.com/IKNL/vantage6) provides this infrastructure. Conceptually, it consists of the following parts:

1. A **central server** that coordinates communication with the nodes;
2. One or more **nodes** \(aka "sites"\) that execute algorithms;
3. **Organisations** that are interested in collaborating with each other;
4. **Collaborations** between organisations;
5. **Users** \(i.e. researchers\) that instruct the nodes which algorithms to execute and which parameters to use;
6. A \(docker\) **registry** that functions as a database of algorithms.

![Simplified overview of the infrastructure](../.gitbook/assets/system-overview.png)

## The server

The central server coordinates communication between users and nodes. It does this by exposing a [RESTful API](https://restfulapi.net) that allows users to create tasks \(essentially these tasks are computation requests\). These tasks are executed by nodes and the results are posted back to the server.

A single server can support multiple organizations, collaborations and users. 

## The Nodes

Nodes perform the heavy lifting in the **vantage6** federated infrastructure. They have access to a site's data store \(e.g. database or CSV file\), receive tasks from the server and execute these. These tasks consist of \(parts of\) an algorithm to run, together with input parameters.

For maximum flexibility, the algorithms are encapsulated in Docker images. A Docker image can be considered a  snapshot of a virtual machine. These images can be run in containers \(image instances\) with their own networking and storage stacks. When run, they use a predefined starting point, for example a script that executes an algorithm. As such, these images can be applied to the local data.

Simplified, when a node receives a task, it:

* downloads \(pulls\) the docker image \(i.e. the algorithm\) associated with the task;
* runs the image in a container, while providing the container with
  * the input parameters from the task
  * access to the node's data store
* captures the output that the algorithm generated; and
* returns the output to the server.

Under the hood, the node performs a few additional checks. For example, it makes sure that a specific image is allowed \(by the collaboration\) and handles decryption/encryption of the input.

