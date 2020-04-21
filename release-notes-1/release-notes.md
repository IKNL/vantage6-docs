---
description: Currently in alpha
---

# 1.0.0 - Trolltunga

* **Updated Command Line Interface \(CLI\)**
  * The commands `vnode list` , `vnode start` and the new command`vnode attach` are aimed to work with multiple nodes at a single machine.
  * System and user-directories can be used to store configurations by using the `--user/--system` options. The node stores them by default at user level, and the server at system level.
  * Current status \(online/offline\) of the nodes can be seen using `vnode list` , which also reports which environments are available per configuration.
  * Developer container has been added which can inject the container with the source. `vnode start --develop [source]`. Note that this Docker image needs to be build in advance from the `development.Dockerfile` and tag `devcon`. 
  * `vnode config_file` has been replaced by `vnode files` which not only outputs the config file location but also the database and log file location.
* **New database model**
  * Improved relations between models. Thereby updating the Python API, see [here](../usage/running-the-server/shell.md).
  * Input for the tasks is now stored in the result table. This was required as the input is encrypted individually for each organization \(end-to-end encryption \(E2EE\) between organizations\).
  * The `Organization` model has been extended with the `public_key` \(String\) field. This field contains the public key from each organization, which is used by the  E2EE module.
  * The `Collaboration` model has been extended with the `encrypted` \(Boolean\) field which keeps track if all messages \(tasks, results\) need to be E2EE for this specific collaboration.
  * The `Task` keeps track of the initiator \(organization\) of the organization. This is required to encrypt the results for the initiator.
* **End to end encryption**
  * All messages between all organizations are by default be encrypted. 
  * Each node requires the private key of the organization as it needs to be able to decrypt incoming messages. The private key should be specified in the configuration file using the `private_key` label. 
  * In case no private key is specified, the node generates a new key an uploads the public key to the server. 
  * If a node starts \(using `vnode start`\), it always checks if the `public_key` on the server matches the private key the node is currently using.
  * In case your organization has multiple nodes running they should all point to the same private key.
  * Users have to encrypt the input and decrypt the output, which can be simplified by using our client `vantage6.client.Client` __for Python __or `vtg::Client` __for R.
  * Algorithms are not concerned about encryption as this is handled at node level.
* **Algorithm container isolation**
  * Containers have no longer an internet connection, but are connected to a private docker network.
  * Master containers can access the central server through a local proxy server which is both connected to the private docker network as the outside world. This proxy server also takes care of the encryption of the messages from the algorithms for the intended receiving organization.
  * In case a single machine hosts multiple nodes, each node is attached to its own private Docker network.
* **Temporary Volumes**
  * Each algorithm mounts temporary volume, which is linked to the node and the `run_id` of the task
  * The mounting target is specified in an environment variable `TEMPORARY_FOLDER`. The algorithm can write anything to this directory.
  * These volumes need to be cleaned manually. \(`docker rm VOLUME_NAME`\) 
  * Successive algorithms only have access to the volume if they share the same `run_id` . Each time a _**user**_ creates a task, a new `run_id` is issued. If you need to share information between containers, you need to do this through a master container. If a master container creates a task, all slave tasks will obtain the same `run_id`.
* **RESTful API**
  * All RESTful API output is HATEOS formatted. **\(**[**wiki**](https://en.wikipedia.org/wiki/HATEOAS)**\)**
* **Local Proxy Server**
  * Algorithm containers no longer receive an internet connection. They can only communicate with the central server through a local proxy service. 
  * It handles encryption for certain endpoints \(i.e. `/task`, the input or `/result` the results\)
* **Dockerized the Node**

  * All node code is run from a Docker container. Build versions can be found at our Docker repository: `harbor.distributedlearning.ai/infrastructure/node` . Specific version can be pulled using tags.
  * For each running node, a Docker volume is created in which the data, input and output is stored. The name of the Docker volume is: `vantage-NODE_NAME-vol` . This volume is shared with all incoming algorithm containers.
  * Each node is attached to the public network and a private network: `vantage-NODE_NAME-net`.

