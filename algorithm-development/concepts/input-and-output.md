---
description: >-
  This section explains the node resources that the algorithm container has
  access to.
---

# Input & output

### File mounts

The algorithm has access to several file mounts:

<details>

<summary>Input </summary>

The input file contains the user defined input. The user specifies this when a task is created.&#x20;

</details>

<details>

<summary>Output </summary>

The algorithm should write its output to this file. When the docker container exits the contents of this file will be send back to the vantage6-server.

</details>

<details>

<summary>Token </summary>

The token file contains a JWT token which can be used by the algorithm to communicate with the central server. The token can only be used to create a new task with the same image, and is only valid while the task has not yet been completed.

</details>

<details>

<summary>Temporary directory</summary>

The temporary directory can be used by an algorithm container to share files with other algorithm containers that:

* run on the same node
* have the same `run_id`

Algorithm containers that origin from another container (a.k.a master container or parent container) share the same `run_id`. i.o. if a user creates a task a new `run_id` is assigned.

</details>

The paths to these files and directories are stored in the environment variables, which we will explain now.&#x20;

### Environment variables

The [environment variables](https://docs.docker.com/engine/reference/run/#env-environment-variables) contain the file paths to the file-mounts. The following environment variables are available:

| Variable           | Description                                                                                                                                                                                                                                                                           |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `INPUT_FILE`       | path to the input file. The _input file_ contains the user defined input for the algorithms.                                                                                                                                                                                          |
| `OUTPUT_FILE`      | Path to the output file. The contents of the _output file_ are send back to the vantage6-server when the algorithm container exits.                                                                                                                                                   |
| `TOKEN_FILE`       | Path to the token file. The _token file_ contains a JWT token which can be used to access the vantage6-server. This way the algorithm container is able to post new tasks and retrieve results.                                                                                       |
| `TEMPORARY_FOLDER` | Path to the temporary folder. This folder can be used to store intermediate results. These intermediate results are shared between all containers that have the same `run_id`. Algorithm containers which are created from an algorithm container themselves share the same `run_id`. |
| `HOST`             | Contains the URL to the vantage6-server.                                                                                                                                                                                                                                              |
| `PORT`             | Contains the port to which the vantage6-server listens. Is used in combination with `HOST` and `API_PATH`.                                                                                                                                                                            |
| `API_PATH`         | Contains the [api base](../../usage/running-the-server/server-configuration.md#configuration-file-structure) path from the vantage6-server.                                                                                                                                           |
| `*_DATABASE_URI`   | Contains the URI of the local database. The  `*`  is replaced by the key specified in the [node configuration](../../usage/running-the-node/configuration.md#configuration-file-structure) file.                                                                                      |

{% hint style="info" %}
[Additional environment](../../usage/running-the-node/configuration.md#configuration-file-structure) variables can be specified in the node configuration file using the `algorithm_env` key. These additional variables are forwarded to all algorithm containers.
{% endhint %}

